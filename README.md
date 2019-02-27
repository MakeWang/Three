# Three
Three 遍历

##目录
* [二叉树遍历方式](#横线)
  * 前序遍历----迭代、栈
  * 中序遍历----迭代、栈
  * 后序遍历----迭代、栈
 * [java代码](#java代码)

  -----------
 # java代码
 ```java
 public class ThreeTest {

    private ThreeNode mRoot;

    public ThreeTest(){
        mRoot = new ThreeNode(1,"A");
    }

    public void createThree(){
        ThreeNode b = new ThreeNode(1,"B");
        ThreeNode c = new ThreeNode(1,"C");
        ThreeNode d = new ThreeNode(1,"D");
        ThreeNode e = new ThreeNode(1,"E");
        ThreeNode f = new ThreeNode(1,"F");
        ThreeNode g = new ThreeNode(1,"G");

        mRoot.leftNode = b;
        mRoot.rightNode = c;
        b.leftNode = d;
        b.rightNode = e;
        e.rightNode = g;
        c.rightNode = f;
    }

    //查询树的深度
    public int getHeight(ThreeNode root){
        if(root == null){
            return 0;
        }
        int l = getHeight(root.leftNode);
        int r = getHeight(root.rightNode);
        return l < r ? r + 1 : l + 1;
    }

    //获取树的节点
    private int getSize(ThreeNode root) {
        if(root == null){
            return 0;
        }
        return getSize(root.leftNode) + getSize(root.rightNode) + 1;
    }

    //前序遍历-----迭代 (中-左-右)
    public void leftSelect(ThreeNode root){
        if(root == null){
            return;
        }
        System.out.println(root.data);
        leftSelect(root.leftNode);
        leftSelect(root.rightNode);
    }
    //前序遍历-----栈 (中-左-右)
    public void leftSelect2(ThreeNode root){
        if(root == null){
            return;
        }
        Stack<ThreeNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()){
            ThreeNode pop = stack.pop();
            if(pop.rightNode != null){
                stack.push(pop.rightNode);
            }
            if(pop.leftNode != null){
                stack.push(pop.leftNode);
            }
            System.out.println(pop.data);
        }
    }

    //中序遍历-----迭代 (左-中-右)
    public void centerSelect(ThreeNode root){
        if(root == null){
            return;
        }
        centerSelect(root.leftNode);
        System.out.println(root.data);
        centerSelect(root.rightNode);
    }

    //中序遍历-----栈 (左-中-右)
    public void centerSelect2(ThreeNode root){
        Stack<ThreeNode> stack = new Stack<>();
        //指针节点
        ThreeNode p = root;
        while (p != null || !stack.isEmpty()){
            while (p != null){
                stack.push(p);
                p = p.leftNode;
            }
            p = stack.pop();
            System.out.println(p.data);
            if(p.rightNode != null){
                p = p.rightNode;
            }else{
                p = null;
            }
        }
    }

    //后序遍历-----迭代 (左-右-中)
    public void rightSelect(ThreeNode root){
        if(root == null){
            return;
        }
        rightSelect(root.leftNode);
        rightSelect(root.rightNode);
        System.out.println(root.data);
    }

    //后序遍历-----栈 (左-右-中)
    public void rightSelect2(ThreeNode root){
        Stack<ThreeNode> stack = new Stack<>();
        ThreeNode p = root;
        while (p != null || !stack.isEmpty()){
            while (p != null){
                stack.push(p);
                if(p.leftNode != null){
                    p = p.leftNode;
                }else{
                    p = p.rightNode;
                }
            }

            p = stack.pop();
            System.out.println(p.data);

            if(!stack.isEmpty() && p == stack.peek().leftNode){
                p = stack.peek().rightNode;
            }else{
                p = null;
            }
        }
    }

    private class ThreeNode{
        private int index;
        private String data;
        private ThreeNode leftNode;
        private ThreeNode rightNode;

        ThreeNode(int index, String data){
            this.index = index;
            this.data = data;
            this.leftNode = null;
            this.rightNode = null;
        }


    }

    public static void main(String []args){
        ThreeTest three = new ThreeTest();
        three.createThree();
        System.out.println("树的深度是："+three.getHeight(three.mRoot));
        System.out.println("树的节点总和是："+three.getSize(three.mRoot));
        //three.leftSelect(three.mRoot);
        //three.leftSelect2(three.mRoot);
        //three.centerSelect(three.mRoot);
        //three.centerSelect2(three.mRoot);
        //three.rightSelect(three.mRoot);
        three.rightSelect2(three.mRoot);

    }

}

 ```
