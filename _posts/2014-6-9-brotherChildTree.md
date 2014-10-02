---
layout: post
category: 算法
title: 兄弟孩子树
tagline: by anping
tags: [兄弟孩子树]
---




树的表示方法很多，双亲儿子，双亲，儿子，儿子兄弟表示法。
但是用来表示树的最多的方式是儿子兄弟表示法。

*	可以用来表示孩子节点不相同的树。
*	不浪费存储空间。


节点类
------

	package sonBrotherTree;

	public class TreeNode {
	private TreeNode sonNode;//这里存储的是最左的儿子节点
	private TreeNode brotherNode;//这里存储的是自己的兄弟节点
	private int value;
	public TreeNode(TreeNode sonNode,TreeNode brotherNode,int value){
	this.sonNode = sonNode;
	this.brotherNode = brotherNode;
	this.value = value;
	}
	public TreeNode getSonNode(){
	return this.sonNode;
	}
	public TreeNode getBrotherNode() {
	return brotherNode;
	}
	public int getValue(){
	return value;
	}

	public void setSonNode(TreeNode sonNode){
	this.sonNode = sonNode;
	}

	public void setBrotherNode(TreeNode brotherNode){
	this.brotherNode = brotherNode;
	}

	}


树类
----


	package sonBrotherTree;
	/**
	*
	* @author anping
	* 主要是对树的实现
	* 不提供删除指定节点的方法，是因为该树的节点是没有指向父亲的
	*/
	public class SonBrotherTree {
	private TreeNode root;//这个是根节点
	public SonBrotherTree(int rootValue){
	//构建树的时候需要吧根节点的值初始化一下
	root= new TreeNode(null,null,rootValue);
	}
	//拿到根节点
	public TreeNode getRoot(){
	return this.root;
	}

	/**
	* 插入儿子节点需要注意的是，假如已经有了儿子节点怎么办？？
	* 没有又怎么办？？
	* 假如没有的话好办，直接插入即可。
	* 但是要是有的话，把原有的儿子变成现在儿子的兄弟
	* @param targetNode
	* @param value
	*/
	public TreeNode insertSonNode(TreeNode targetNode,int value){
	TreeNode nowSonNode = new TreeNode(null, null, value);
	TreeNode preSonNode = targetNode.getSonNode();
	if(preSonNode!=null){
	//先见前儿子变成现在儿子的兄弟,然后在添加儿子
	nowSonNode.setBrotherNode(preSonNode);
	}
	targetNode.setSonNode(nowSonNode);
	return nowSonNode;
	}

	/**
	* 插入的兄弟要是有兄弟怎么办？吧现在的兄弟插入。而原有的兄弟变成现在兄弟的兄弟
	* 如果没有，那么直接插入
	* @param targetNode
	* @param value
	*/
	public TreeNode insertBrotherNode(TreeNode targetNode ,int value){
	TreeNode nowBrotherNode = new TreeNode(null, null, value);
	TreeNode preBrotherNode = targetNode.getBrotherNode();
	if(preBrotherNode!=null){
	nowBrotherNode.setBrotherNode(targetNode.getBrotherNode());
	}
	targetNode.setBrotherNode(nowBrotherNode);
	return nowBrotherNode;
	}

	/**
	* 删除兄弟的时候如果只有这个兄弟那么先把兄弟的子孙全部删除，在删除该兄弟
	* 如果还有其他的兄弟，那么需要保留其他的兄弟。
	*/
	public void deleteBrotherNode(TreeNode targetNode){
	if(targetNode!=null &&targetNode.getBrotherNode()!=null)
	{
	this.destoryNode(targetNode.getBrotherNode());
	}
	}
	/**
	* 删除儿子的时候如果儿子不是null 的先吧儿子的子孙全删掉
	* @param sonNode
	*/
	public void deleteSonNode(TreeNode targetNode){
	if(targetNode!=null && targetNode.getSonNode()!=null){
	this.destoryNode(targetNode.getSonNode() );
	}
	}
	/**
	* 通过遍历整颗树来拿取数据
	* @param value
	* @return
	*/
	public TreeNode searchValue(int value){

	return this.searchValue(root, value);
	}

	/**
	* 使用递归取出值，递归用的少，真的是不熟。
	* @param node
	* @param value
	* @return
	*/
	public TreeNode searchValue(TreeNode node,int value){
	if(node.getValue()==value){
	return node;
	}
	if (node.getValue()!=value&&node.getSonNode()!=null){

	return searchValue(node.getSonNode(), value);
	}
	if(node.getValue()!=value&&node.getBrotherNode()!=null){

	return searchValue(node.getBrotherNode(), value);
	}
	return null;

	}
	/**
	* 已图形的方式遍历出来
	*/

		public static void main(String[] args) {
		SonBrotherTree tree = new SonBrotherTree(12);
		TreeNode rootSon = tree.insertSonNode(tree.getRoot(), 100);
		TreeNode rootSonBrother = tree.insertBrotherNode(rootSon, 122);
		tree.insertSonNode(tree.getRoot(),101);
		tree.insertSonNode(rootSonBrother,101);

		System.out.println(tree.getRoot().getValue()+”–”+tree.getRoot().getSonNode().getValue()+”—”+tree.getRoot().getSonNode().getBrotherNode().getValue());

		TreeNode search = tree.searchValue(101);
		System.out.println(search);

		}
		private void destoryNode(TreeNode node){
		if(node.getSonNode()!=null){
		destoryNode(node.getSonNode());
		}
		if(node.getBrotherNode()!=null){
		destoryNode(node.getBrotherNode());
		}
		node.notify();
		node=null;
		}

	}



