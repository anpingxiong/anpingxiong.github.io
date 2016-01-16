---
layout: post
category: android
title: SpannableString的学习总结
tagline: by anping
tags: [SpannableString]
---

SpannableString
----------------


我理解的SpannableString
======================

SpannableString是文字的扩展工具，能够在android中让文字的显示的更加有趣，比如给文件设置下划线，字体风格，加链接，将指定的文件替换成图片，比如
米聊的表情，等等.


一个简单的例子
===========

    TextView textView  = (TextView) findViewById(R.id.spantext);

    SpannableString spanText = new SpannableString("StrikethroughSpan");

    spanText.setSpan(new UnderlineSpan(), 0, spanText.length(), Spannable.SPAN_INCLUSIVE_EXCLUSIVE);

    spanText.setSpan(new URLSpan("http://www.baidu.com"), 10, spanText.length(), Spannable.SPAN_INCLUSIVE_EXCLUSIVE);

    String data = spanText.toString();

    textView.setText(spanText);

    textView.setMovementMethod(new LinkMovementMethod());


上面的例子是给StrikethroughSpan 这个字符串加了底部的下划线，并且将throughSpan字符设置了可以点击的URL,但是textView一般是不响应点击事件的，所有如果需要点击文字跳转到链接的话则需要设置textView 为可点击状态.--->textView.setMovementMethod(new LinkMovementMethod());

    textView.setMovementMethod(new LinkMovementMethod()); //它做的事情便是让textView可以点击
    //源代码做了这么件事
    setFocusable(true);
    setClickable(true);
    setLongClickable(true);


android提供了哪些Span
===================


    AbsoluteSizeSpan 指定文字大小
    TypefaceSpan 可以设置不同的字体
    AlignmentSpan.Standard 标准文本对齐
    BackgroundColorSpan 文本背景颜色
    ForegroundColorSpan 文字字体颜色
    LeadingMarginSpan 文本缩进
    TabStopSpan 制表位偏移样式
    TextAppearanceSpan 使用style文件来定义文本样式
    RelativeSizeSpan 对于文本设定的大小的相对比例
    ScaleXSpan 将字体按比例进行横向缩放
    URLSpan 可以打开一个链接
    StyleSpan 正常、粗体、斜体和同时加粗倾斜四种样式
    StrikethroughSpan 删除线样式
    QuoteSpan 在文本左侧添加一条表示引用的竖线
    UnderlineSpan 给一段文字加上下划线
    SubscriptSpan 脚注样式，比如化学式的常见写法
    SuperscriptSpan 上标样式，比如数学上的次方运算
    BulletSpan 文本着重样式，类似于HTML中的
    标签的圆点效果
    DrawableMarginSpan 、IconMarginSpan 图片+Margin样式
    ImageSpan 图片样式，主要用于在文本中插入图片 聊天中的emoji表情显示用的就是这个
    MaskFilterSpan 文本滤镜 目前只有模糊效果和浮雕效果
    RasterizerSpan 光栅化

简单的描述下米聊的普通表情实现
=========================

米聊的普通的表情实现实际上使用的就是adroid 提供的ImageSpan.
消息发送过程中表情的传输实际上使用的是特殊协议字符串，比如[midoge],但是由于版本可能不兼容问题，在某一些老版本的米聊中会显示不出来表情，这个时候就会显示早定义号的中文字符。

一般我们会准备四样东西。

    【midoge】【米狗】[midoge]　图片
    【midoge】表示如果客户端版本语言是英文并且不支持该表情时需要显示的字符串.
    【米狗】表示如果客户端语言版本是中文但是不支持该表情时需要显示的字符串.
    [midoge]表示消息传输过程中的定义的协议字符串
    图片　表示　表情图

![Alt text](http://anpingxiong.github.io/image/smilywork.png)


简单描述下URLSpan如何扩展
======================

URLSpan　关联了连接之后可以点击跳转到浏览器，但是我们可以扩展它，用来跳转我们自己定义的activity.
比如米聊　聊天的时候发送 "1234567".点击之后会弹出菜单供选择是需要跳转到公会还是单人界面.

原理是继承了URLSpan 类并且重新了onCick 事件，而在onClick中调用的是我们自己写好的跳转。比如：


    @Override
       public void onClick(View widget) {
            Intent intent = new Intent(~~,~~~.class);
            intent.setExtra(..,..);
            ~~.startActivity(intent);
       }
