---
title: data_structure
date: 2022-07-08 15:40:09
tags:
- computer science
categories:
- learn in NJUST
---

* 数据结构：
  
  1. 集合
  2. 线性结构
  3. 树形结构
  4. 图状结构或网状结构
  
  * 逻辑结构：数据元素之间逻辑关系的整体
  * 物理结构：数据结构在计算机中的表示（即映像）
  
  
  
  通常数据元素在计算机中有两种不同的表示方法：
  
  * 顺序映像：借助数据元素在存储器中的仙姑地位之来表示数据元素之间的逻辑关系，所以顺序存储就是把逻辑上相邻的数据元素存储在物理位置相邻的存储单元中
  * 非顺序映像：借助指示数据元素存储地址的指针表示数据元素之间的逻辑关系，所以链式存储就是用一组任意的存储单元存储数据元素
  
------
  
<!-- more -->

  # 线性表
  
  ## 线性表的类型定义
  
  **线性表**：由n个数据元素（节点）$a_1,a_2,...,a_n$组成的有限序列。当n=0时成为空表，常常将非空的线性表记作：$(a_1,a_2,...a_n)$
  
  * 对非空的线性表，有且仅有一个开始结点$a_1$，它没有直接前趋，而今有一个直接后继$a_2$
  * 有且仅有一个终端结点$a_n$，它没有直接后继，而今有一个直接前趋$a_{n-1}$
  * 其余的内部节点$a_i(2\leq i\leq n-1)$都有且仅有一个直接前趋$a_{i-1}$和一个直接后继$a_{i+1}$
  
  ## 线性表的顺序表示和实现
  
  顺序表：把线性表的结点按逻辑顺序依次存放在一组地址连续的存储单元里
  
  $Loc(a_i)=(i-1)*m+Loc(a_1)=Loc(a_1)-m+i*m$
  
  
  
  顺序表的类定义1（采用方式一定义的一维数组）:
  
  ```c++
  const int MaxSize = 100;
  typedef int ElemType;　//ElemType代表数据元素的类型
  class SqList_s{  //顺序表类SqList_s
  　　private:
  　　ElemType elem_array[MaxSize]; //一维数组的容量，MaxSize为可存放的数据元素的最大个数
  　　int length；
  　　public：
  　　　　…//12个基本操作对应的成员函数
  };
  ```
  
  顺序表的类定义2（采用方式二定义的一维数组）：
  
  ```c++
  const int LISTINCREMENT=10;   
  typedef int ElemType;　//ElemType代表数据元素的类型
  class SqList_d{　//顺序表类SqList_d
  　　private:
  　　ElemType *elem; 	//指针elem指向一维数组的第一个元素
  　　int length;		//线性表的长度，表中数据元素的实际个数
  　　int MaxSize; 
  　　public：
  　　…//12个基本操作对应的成员函数
  };
  ```
  
  初始化顺序表
  
  ```c++
  SqList_d::SqList_d(int n){
  	elem = new int[n];
      length = 0;
      MaxSize = n;
  }
  //O(n)
  ```
  
  销毁顺序表
  
  ```c++
  SqLsit_d::~SqList_d(){
      delete [] elem;
      length = 0;
      MaxSize = 0;
  }
  //O(1)
  ```
  
  顺序插入数据元素
  
  ```c++
  SqList_d::SqList_insert(int i,int e){// 在第i个位置插入数据元素
  	if(i<1||i>length+1) return; //插入位置异常
  	if(length>=MaxSize){//顺序表已满,分配增量空间
          int *elem1=new [MaxSize+LISTINCREMENT];
      	for(int i=0;i<length;i++)
              elem1[i]=elem[i];
          delete elem[];
          elem=elem1;
          MaxSize+=LISTINCREMENT;
          int *p=&elem[i-1],*q=&elem[length-1];
          for(;q>=p;q--)
              *(q+1)=*q;
          *p=e;
          length++;
      }
  }
  //O(n)
  ```
  
  顺序表删除数据元素
  
  ```c++
  SqList_d::SqList_delete(int i,int &e){// 在删除第i个数据元素
      if(i<1||i>length) return; //插入位置异常
  　 int *p=&elem[i-1],*q=&elem[length-1];
       e=*p; 
       for(;p<q;p++) *p=*(p+1);
       length--;
  }
  //O(n)
  ```
  
  顺序表返回数据元素的位置
  
  ```c++
  int SqList_d::Locatex (int x){// 数据元素定位，若找到，返回该数据元素
                                                      //在表中的位序；末找到，返回0。
       for(int i=0;i<length;i++)
          if(elem[i]==x)  return i+1;
       return 0;
    }	
  ```
  
  特点：
  
  1. 节省存储空间。由于结点之间的相邻逻辑关系可以用物理位置上的相邻关系表示，因此不需增加额外的存储空间来表示此关系（如链表则需利用指针来表示逻辑相邻关系）
  2. 随机存取
  3. 插入和删除操作需要移动大量的数据
  
  ## 线性表的链式表示和实现
  
  ### 线性链表
  
  		链表是指用一组任意的存储单元来依次存放线性表的结点，这组存储单元即可以是连续的，也可以是不连续的，甚至是零散分布在内存中的任意位置上的。因此，链表中结点的逻辑次序和物理次序不一定相同。为了能正确表示结点间的逻辑关系，在存储每个结点值的同时，还必须存储指示其后继结点的地址（或位置）信息，这个信息称为指针(pointer)或链(link)。这两部分组成了链表中的结点结构：
  
  1. 存储数据元素自身新写的部分，称为数据域
  2. 存储与前驱或后继结点的逻辑关系，称为指针域
  
  单链表的类定义
  
  ```c++
  typedef int ElemType;	  //ElemType代表数据元素的类型
  struct Node{
     ElemType data;
     Node *next;      
  };
  class LinkList{
     private:
     Node *Head;
     public:
         LinkList() ;//构造一个空的线性表L
        ~LinkList();//销毁线性表L
        void CreateList1(int n);//头插法创建具有n个数据元素的线性链表
        void CreateList2(int n);//尾插法创建具有n个数据元素的线性链表
        void ListInsert(int i,int e);//在表中第i个位置插入数据元素
        int ListDelete(int i);//删除表中第i个位置上的数据元素
        int GetElem(int i);//获取第i个数据元素的
        int LocateElem(int e);//在链表中查找是否存在数据元素e
        int ListLength();//计算表长
  };
  ```
  
  
  
  插入运算：首先找到$a_{i-1}$的存储位置p，然后生成一个数据域为x的新结点，并令q指针指向该新结点，新结点的指针域指向结点$a_i$。
  
  ```c++
  void  insertnode(head, x, i)
  {
  	LNode  * p，*q;
      p=getnode(head，i-1);
      if(p==NULL){
          cout<<“position  error”<<endl;
  	return ERROR;
      }
      q=new LNode;
  	q–>data=x;
  	q–>next=p–>next;
  	p–>next=q;
  }
  ```
  
  
  
  删除运算“首先找到$a_{i-1}$的存储位置p，然后令$p\to next$指向$a_i$的直接后继结点，最后释放结点$a_i$的空间。
  
  ```c++
  void  deletelist(head, i)
  {
      LNode *p， *r;
      p=getnode(head，i-1);
      if(p= =NULL || p–>next= =NULL)
          return ERROR;
      r=p–>next;
      p–>next=r–>next;
      delete r;
  }
  ```
  
  
  
  按序号查找：从链表的头指针出发，顺链域next逐个结点往下搜素，直到搜索到第i个结点为止。
  
  ```c++
  LNode  * getnode( head ，i)
  //在链表head中取第i个数据链表有头结点
  {
      p=head;
      j=0;  //计数用
      while(p–>next && j<i){
          p=p–>next;
          j++;
      }
      if (i==j)
          return p;
      else return NULL;
  }
  ```
  
  **静态链表**：用一维数组来实现线性链表，这种用一维数组表示的线性链表，称为静态链表。
  
  ---
  
  ### 循环链表
  
  循环链表是一种头尾相接的链表。其特点是无需增加存储量，仅对表的链接方式稍作改变，即可使得表处理更加方便灵活。由于循环链表中没有NULL指针，都涉及遍历操作时，其终止条件为判断它们是否等于某一指定指针，如头指针或尾指针等。
  
  ---
  
  ### 双向链表
  
  **双向链表**：在单链表的每个结点里再增加一个指向其直接前趋的指针域prior，这样就形成的链表中有里那个方向不同的链，称为双向链表。
  
  ```c++
  struct DuLNode{
      datatype data;
      DuLNode *prior, *next;
  };
  ```
  
  
  
  ---
  
  ### 一元多项式的表示和相加
  
  用线性链表表示。增加头结点，每个结点有coef（系数），exp（指数），next（指针），其中头结点的exp为-1。
  
  相加运算规则：指数相同的系数相加。
  
  步骤：分别对两个链表ha、hb进行扫描，设工作指针pa、pb分别指向两个多项式当前进行比较的结点，q指针指向pa的前趋，初始：q = ha， pa = ha->next， pb = hb->next
  
  * 若pa，pb都不为空，则比较两者指数
  * * pa系数 > pb系数：q，pa后移
    * pa系数 = pb系数：将pb所知节点的系数加到pa所指结点的系数上；若和为0，则pa所指结点删除。q，pa，pb调整
    * pa系数 < pb系数：从表hb中复制pb所指节点的coef，exp，，并将其插入到ha表pa所指结点之前
  * 若pb不空：hb表中从pb开始的结点插入到ha表尾上
  
  ```c++
  int comp(int a,int b){
      if(a>b) 
          return -1;
      if(a==b) 
          return 0;
      else 
          return 1;  }
  
  PloyAdd(ha,hb){
      q=ha;
      pa=ha->next;  
      pb=hb->next;
      while(pa!=NULL && pb!=NULL)
          switch(comp(pa->exp,pb->exp)){
                  -1:    q=pa; pa=pa->next;
                  break;
                  0:   pa->coef+=pb->coef;
                  if(pa->coef==0) { 
                      q->next=pa->next;
                      delete pa;
                      pa=q;
                  }
                  else q=pa;
                  pa=pa->next;
                  pb=pb->next;
                  break;
                  1:       r=new  Node;
                  r->coef=pb->coef;
                  r->exp=pb->exp;
                  q->next=r;
                  r->next=pa;
                  q=r;
                  pb=pb->next;
                  break
          }
      while(pb!=NULL){  
          r=new Node;
          r->coef=pb->coef;
          r->exp=pb->exp;
          q->next=r;
          r->next=pa;
          q=r;
          pb=pb->next; 
      }
  }
  ```
  
  ---
  
  # 栈和队列
  
  ## 栈
  
  **栈（*stack*）**是限制在表的一端进行插入和删除运算线性表，通常称插入、删除的这一端为**栈顶**，另一端为**栈底**。栈的修改是按后进先出的原则进行的。
  
  ### 顺序存储栈
  
  栈的顺序存储结构简称为顺序栈，可用数组来实现顺序栈。因为栈底位置是固定不变的，所以可以将栈底位置设置在数组的两端的任何一个断电；栈顶位置是随着进栈和退栈操作二变化的，故需用一个整形变量**top（栈顶指针）**来指出栈顶。一般使用数组的最后一个元素。
  
  顺序栈的定义和基本实现
  
  ```c++
  class SqStack{
      private:
      int *base;//栈底指针
      int top;//栈顶
      int stacksize;//栈容量
      public:
      SqStack(int m);//构建一个长度为m的栈
      ~SqStack(){delete [] base;
                 top=-1;
                 stacksize=0;
                }//栈销毁
      void push(int e);//入栈
      int pop(int &e);//出栈
      int getTop();//获取栈顶元素
      int stackEmpty();//测栈空
      void stackTranverse();//显示栈中元素
  };
  ```
  
  ### 栈的链式存储结构及实现
  
  栈的链式存储结构称为栈链，插入和删除操作仅限制在链头位置上进行，栈顶指针就是链表的头指针。
  
  ```c++
  Struct Node{
      int data;
      Node *next;
  };
  class LinkStack{
      private:
      Node *top;                      //栈顶指针即链栈的头指针
      public:
      LinkStack(){top =NULL;}         //构造函数，置空链栈
      ~LinkStack();                   //析构函数，释放链栈中各结点的存储空间
      void push(int e);                 //将元素e入栈
      void pop(int &e);                       //将栈顶元素出栈
      int getTop() {
          if(top!=NULL) 
              return top->data;
      }  //取栈顶元素
      int empty(){
          top==NULL? return 1：return 0;
      }   //判断链栈是否为空栈
  };
   
  LinkStack::~LinkStack(){
     while(top){
        p=top->next;
        delete top;
        top=p;
     }
  }
  
  void LinkStack::push(int e){
        s = new Node; 
        if(!s){ 
          cout<<“内存分配失败”;
          return;
         }
      s->data =e;
      s->next = top;
      top = s;
   }
  
  void LinkStack::pop(int &e){
       if (top==NULL){
           cout<<“溢出”;
           return ;
       }
      e=top->data;       //暂存栈顶元素
      p=top; 
      top = top->next;  
      delete p;
  }
  ```
  
  ---
  
  ## 队列
  
  队列是线性表的特例。它将元素排列成队，有入口（队尾）和出口（队头），数据元素只能从队尾入队，从队头离队，FIFO和LILO。
  
  ### 循环队列
  
  将存储队列的数组看成是头尾相接的圆环，并成为循环存储空间，即允许队列直接从数组中下标最大的位置延续到下标最小的位置，这个操作可以通过取模运算实现。队列的这种头尾相接的顺序存储结构称为**循环队列**（***Circular Queue***）。
  
  在循环队列中进行入队、出队操作时，头尾指针仍然需要加1，朝前移动。
  
  循环队列定义：
  
  ```c++
  class CQueue {
      private:
  	int *base;// 存储空间基址
  	int front;// 队头指针
  	int rear;// 队尾指针
  	int queuesize;// 队容量
      public:
      CQueue(){base=new int[queuesize]; front=rear=0;}//构造空队列
  	~CQueue();// 析构函数，释放链队各结点的存储空间
  	void EnQueue(int e);// 元素e入队
  	void DeQueue(int &e);// 队顶元素出队
  	int GetHead();// 取队头元素
  	int GetLast();//取队尾元素
  	void QueueDisplay();//遍历队，输出队列元素
  };
  ```
  
  循环队列入队算法：
  
  ```c++
  void CQueue::EnQueue(int e){
      if((rear+1)%(queuesize) == front){
          cout<<"上溢，无法入队";
          return;
      }
      base[rear]=e;
      rear=(rear+1)%queuesize;
  }
  ```
  
  循环队列出队算法：
  
  ```c++
  void DeQueue(int &e){
      if(front == rear){
          cout<<"下溢，不能出队";
          return ;
      }
      e=base[front];
      front=(front+1)%queuesize;
  }
  ```
  
  ### 链队列
  
  队列的链式存储结构称为**链队列**（***Linked Queue***）。根据队列先进先出的特性，链队列是仅在表头删除元素和表尾插入元素的单链表。为了操作方便，链队列用有头结点的链表表示，并设置队头指针指向链队列的头结点，队尾指针指向终端结点。
  
  链队列的类定义和基本操作：
  
  ```c++
  struct Node{
     int data;
     Node *next;
   };
  class LinkQueue{
      private:
      Node *front;//队头指针，链表头为队头
      Node *rear;//队尾指针，链表尾为队尾
      public:
      LinkQueue(){front=rear=new Node;front->next=NULL;}//构造空队列
      ~LinkQueue();// 析构函数，释放链队各结点的存储空间
      void EnQueue(int e);// 元素x入队
      void DeQueue(int &e);// 队顶元素出队
      int emptyQueue(){if(front==rear)return 1;else return 0;}//队列判空
      int GetHead();// 取队头元素
      int GetLast();//取队尾元素
      void QueueDisplay();
  };
  ```
  
  链队列入队算法：
  
  ```c++
  void LinkQueue::EnQueue(int e){
      Node *s;
      s=new Node; s->data=e; 
      s->next=rear->next;
      rear->next=s; 
      rear=s; 
  }
  ```
  
  链队列出队算法：
  
  ```c++
  void LinkQueue::DeQueue(int &e){
      Node *p;
      if(rear==front){
          cout<<"下溢";return;
      }//队空，则下溢
      p=front->next;
      e=p->data;
      front->next=p->next;
      if(p->next==NULL)
          rear=front;
      delete p;
  }
  ```
  
  ---
  
  ## 串
  
  ### 串类型的定义
  
  **串**(***String***)是零个或多个字符组成的有限序列。一般记作$S=“a_1a_2a_3…a_n”$，其中S是串名，双引号括起来的字符序列是串值；$a_i(1\leq i\leq n)$可以是字母、数字或其它字符。
  
  空白串：由空格组成
  
  空串：长度为零
  
  **子串**：串中任意个连续字符组成的子序列，包含子串的串称为**主串**
  
  通常将子串在主串中首次出现时的该子串的首字符在主串中的位置，定义为子串在主串中的序号（或位置）。
  
  * 串长：`int strlen(char *s);`
  * 串复制：`char *strcpy(char *to, char *from)`
  * 串连接：`char strcat(char *to, char *from)`
  
  求子串：
  
  ```c++
  void substr(string sub,string s,int pos,int len){
      if(pos<0 || pos>strlen(s)-1 || len<0)
          return ;
      strncpy(sub,s+pos,len);
  }
  ```
  
  串的定位：
  
  ```c++
  index(s, t, pos){
      if(pos>0){
          n=strlen(s);    
          m=strlen(t);  
          i=pos;
          while(i<n-m+1){
              substr(sub,s,i,m);
              if(strcmp(sub,t)!=0)
                  ++i;
              else return i;
          }
      }
      return  0;
  }
  ```
  
  
  
  ### 串的表示和实现
  
  #### 定长顺序存储表示
  
  定长顺序存储表示，也称为静态存储分配的顺序表，数组的上界预先给出
  
  ```c++
  #define MAXSTRLEN 256
  typedef char sstring[MAXSTRLEN];
  sstring s;   //s是一个可容纳255个字符的顺序串。
  ```
  
  #### 堆分配存储表示
  
  这种存储表示的特点是，仍以一组地址连续的存储单元存放串值字符序列，但它们的存储空间是在程序执行过程中动态分配而得。
  
  #### 串的链式存储结构
  
  可用单链表方式来存储串值，串的这种链式存储结构简称为链串。一个链串由头指针唯一确定。
  
  ---
  
  ### 串的模式匹配算法
  
  一般将主串称为目标串，子串称为模式串
  
  #### Brute Force算法
  
  ```c++
  //在主串s中定位子串t,不存在返回-1 
  int Index(char s[],char t[]){
      int i=0,j=0;
      int slen=strlen(s)-1;//s最大下标号
      int tlen=strlen(t)-1; //t最大下标号
      while(i<=slen && j<=tlen){
          if(s[i]==t[j]){ 
              ++i; 
              ++j;
          }
          else{ 
              i=i-j+1;
              j=0;  
          }
      }
      if(j>tlen)
          return i-tlen-1;
      else 
          return -1;
  }
  ```
  
  #### KMP算法
  
  失败函数
  $$
  f[j]=\left\{
  \begin{aligned}
  i \quad 使p_1...p_i = p_{j-1+1}...p_j\\
  0 \quad 其它
  \end{aligned}
  \right.
  $$
  
  $$
  next[j]=\left\{
  \begin{aligned}
  0\quad 当j=1时 \\
  f[j-1]+1
  \end{aligned}
  \right.
  $$
  
  $next[i]$给出了当与模式串的第i个字符不匹配时，应该重新开始比较的字符序号。
  
  ```c++
  int Index_KMP(s,t){ //下标计数从1开始
     i=1;j=1;
     while(i<=s串长&&j<=t串长){ 
         if(j= =0 || s[i]= =t[j]){ 
             i++;
             j++;}
         else 
             j=next[j];
         if(j>t串长) 
             return i-t串长;
         else
             return 0;
  }
  ```
  
  ---
  
  # 数组
  
  * 行优先
  * 列优先
  
  按上述两种方式顺序存储的序组，只要知道开始结点的存放地址（即基地址），维数和每维的上、下界，以及每个数组元素所占用的单元数，就可以将数组元素的存放地址表示为其下标的线性函数。
  
  ## 矩阵
  
  ### 对称矩阵、三角矩阵
  
  对称矩阵中的元素关于主对角线对称，故只要存储矩阵中上三角或下三角中的元素，这样，能节约近一半的存储空间。不失一般性，我们按“行优先顺序”存储主对角线（包括对角线）以下的元素。
  $$
  K=\left\{
  \begin{aligned}
  i(i-1)/2+j,\quad j \geq j \\
  j(j-1)/2+i,\quad i < j 
  \end{aligned}
  \right.
  $$
  
  ---
  
  ### 稀疏矩阵
  
  设矩阵A中有s个非零元素，若s远远小于矩阵元素的总数（即$s\leq m*n$），则称A为稀疏矩阵
  
  有s个非零元素。令 e=s/(m*n)，称e为矩阵的稀疏因子。通常认为e≦0.05时称之为稀疏矩阵。在存储稀疏矩阵时，为了节省存储单元，不存储0元素。由于非零元素的分布一般是没有规律的，因此在存储非零元素的同时，必须同时记下它所在的行和列的位置(i,j)。
  所以，一个三元组$(i,j,a_{ij})$唯一确定了矩阵A的一个非零元。因此，稀疏矩阵可由表示非零元的三元组及其行列数唯一确定
  
  ```c++
  \\三元顺序表
  #define MAXSIZE 10000
    typedef int datatype;
     struct triple{
        int   i,j;
        datatype v;
     } ;
  struct tripletable{
       triple  data[MAXSIZE];
       int  m,n,t;
       }tripletable;
  
  ```
  
  ```c++
  \\转置算法
  TransposeSMatrix(M, T){
      T.mu=M.nu;
      T.nu=M.mu; 
      T.tu=M.tu;
      if(T.tu){
          q=0； // q为当前三元组在T.data[ ]存储位置(下标)
          for (col=1; col<=M.nu;  ++col)
              for (p=0;  p<=M.tu-1;  ++p) //p为扫描M.data[ ]的“指示器”
                  if (M.data[p].j= =col){
                      T.data[q].i =M.data[p].j;
                      T.data[q].j=M.data[p].i;
                      T.data[q].e=M.data[p].e; ++q;
                  }
      }
  }//  TransposeSMtrix
  ```
  
  下面给出另外一种称之为快速转置的算法，其算法思想为：对A扫描一次，按A第二列提供的列号一次确定位置装入B的一个三元组。具体实施如下：一遍扫描先确定三元组的位置关系，二次扫描由位置关系装入三元组。可见，位置关系是此种算法的关键。
  
  ```c++
  FastTransposeSMatrix(M, T){
      T.mu=M.nu;
      T.nu=M.mu;
      T.tu=M.tu;
      if(T.tu){
          for (col=1; col<=M.nu; ++col) 
              num[col]=0;
          for (t=1; t<=M.tu; ++t) 
              ++num[M.data[t].j];
          pos[1]=1;
          for(col=2; col<=M.tu;  ++col)
              pos[col]=pos[col-1]+num[col-1];
          for(p=1; p<M.tu;++p) {
              col=M.data[p].j;
              q=pos[col]-1;
              T.data[q].i=M.data[p].j;
              T.data[q].j=M.data[p].i;
              T.data[q].e=M.data[p].e;  
              ++pos[col];
          }//for
      }//if
  }//FastTransposeSMatrix
  ```
  
  ---
  
  ## 广义表
  
  广义表是n(n>=0)个元素$a_1,a_2,a_3,…,a_n$的有限序列，其中$a_i$或者是原子项，或者是一个广义表。通常记作$LS=（a_1,a_2,a_3,…,a_n)$。LS是广义表的名字，n为它的长度。若$a_i$是广义表，则称它为LS的子表。
  
  通常用圆括号将广义表括起来，用逗号分隔其中的元素。为了区别原子和广义表，书写时用大写字母表示广义表，用小写字母表示原子。若广义表$LS(n\geq 1)$非空，则a1是LS的表头，其余元素组成的表$(a_2,…a_n)$称为LS的表尾。
  
  ---
  
  # 树和二叉树
  
  树型结构是结点之间有分支，且具有层次结构的结构
  树的定义：树是$n(n\geq 0)$个结点的有限集T，T为空时称为空树，否则满足：
  
  1. 有且仅有一个特定的称为根的结点
  2. 其余的结点可分为$m(m\geq 0)$个互不相交的子集，其中每个子集又是一棵树，并称之为子树
  
  # 树
  
  * 结点
  * 结点的度：结点拥有的子树的个数
  * 树的度：树中所有结点的度的最大值
  * 叶子结点：度为0的结点
  * 分支结点：度不为0的结点
  * 孩子结点：结点x的子树的根结点
  * 双亲结点
  * 兄弟结点
  * 堂兄弟结点：层次相同，双亲结点不同
  * 树的深度：树中距离根最远的结点所处的层次
  * 树的高度：从下往上定义
  
  # 二叉树
  
  定义：二叉树是由n个结点的有限集合构成，此集合或者为空集，或者由根结点及两棵互不相交的左右子树组成，并且左右子树都是二叉树
  
  - 性质一：在二叉树的第$i(i\geq 1)$层上至多有$2^{i-1}$个结点
  - 性质二：深度为$k(k\geq 1)$的二叉树至多有$2^k-1$个结点
  - 性质三：对任何一棵非空二叉树，如果其叶子结点数为$n_0$，度为2的结点为$n_2$，则$n_0=n_2+1$
  
  满二叉树：深度为k且有$2^k-1$个结点的二叉树
  完全二叉树：如果一棵深度为k的二叉树且具有n个结点的二叉树，它的每一个结点的都与深度为k的满二叉树中顺序编号为1～n的结点一一对应
  
  - 性质四：具有n(n>0)个结点的完全二叉树的深度为$\lfloor log_2n\rfloor+1$
  - 性质五：如果对一棵有n个结点的完全二叉树的结点按层次编号，对任意结点i，有：
  
  1. 如果i=1,则结点i无双亲，是结点的根；如果i>1，则其双亲的编号为i/2（取整）
  2. 如果$2i>n$，无左孩子；否则，其左孩子是结点$2i$
  3. 如果$2i+1>n$，则结点i无右孩子；否则，其右孩子结点为$2i+1$
  
  ## 二叉树的存储结构
  
  ### 顺序存储结构
  
  按照顺序存储的定义，用一组地址连续的存储单元依次自上而下、自左至右存储完全二叉树上的结点元素，存储时只保存各结点的值。仅仅适用于满或完全二叉树，结点之间的层次关系由性质5确定
  
  设有一棵一般的二叉树，如下图所示，需要将它放在一个一维数组中。为了实现对某个结点的查找和定位，也需仿照完全二叉树那样，对二叉树的结点进行顺序编号。在编号时，增加一些并不存在的空结点并按顺序对其进行编号处理，使之成为一棵与原二叉树高度相同的完全二叉树的形式。
  
  ### 链式存储结构
  
  根据二叉树的定义，二叉树的每个结点可以有两个分支，分别指向左右子树。因此，二叉树的结点至少包含三个域：`data`、`lchild`、`rchild`
  
  ```c++
  typedef struct BiTNode{//二叉树的结点类型
      ElemType data ;//数据域
      BiTNode* lchild;//指向左子树的指针
      BiTNode* rchild;//指向右子树的指针
  }BiTNode;
  ```
  
  ### 遍历二叉树
  
  若规定先左后右，则只有前三种情况，分别规定为：DLR-先（根）序遍历;LDR-中（根）序遍历;LRD-后（根）序遍历
  
  1、先序遍历二叉树的操作定义为：
  若二叉树不空，则：
  
  1. 访问根结点；
  2. 先序遍历左子树；
  3. 先序遍历右子树。
  
  2、中序遍历二叉树的操作定义为：
  若二叉树不空，则：
  
  1. 中序遍历左子树；
  2. 访问根结点；
  3. 中序遍历右子树。
  
  3、后序遍历二叉树的操作定义为：
  若二叉树不空，则：
  
  1. 后序遍历左子树；
  2. 后序遍历右子树；
  3. 访问根结点。
  
  ```c++
  //二叉链表的类定义：
  typedef struct BiTNode{//二叉树的结点类型
        ElemType data ;//数据域
        BiTNode* lchild;//指向左子树的指针
        BiTNode* rchild;//指向右子树的指针
   }BiTNode;
  class BinaryTree{
      private:
         BiTNode *bt;
      public: 
        BinaryTree(){bt=NULL;}// 构造函数,将根结点置空
        void CreateBiTree(BiTNode *&t);// 创建一棵二叉树
        void PreOrder (BiTnode *p);// 递归算法：先序遍历二叉树
        void InOrder (BiTnode *p);// 递归算法：中序遍历二叉树
        void PostOrder (BiTnode *p);// 递归算法：后序遍历二叉树
        int countleaf(BiTnode *p)//递归算法：计算树叶数
        int hight(BiTNode *p) //递归算法：计算树高
        …….
        	
  };
  ```
  
  ```c++
  //先序
  void BinaryTree::preorder(p){
    If(p != NULL){
      cout<<p->data;
      preorder(p->lchild);
      preorder(p->rchild);
    }
  }
  ```
  
  ```c++
  //中序
  void BinaryTree::inorder(p){
    If(p){
      inorder(p->lchild);
      cout<<p->data;
      inorder(p->rchild);
    }
  }
  ```
  
  ```c++
  //后序
  void BinaryTree::postorder(p){
    If(p){
      postorder(p->lchild);
      postorder(p->rchild);
      cout<<p->data;
    }
  }
  ```
  
  ```c++
  \\叶子计数
  int BinaryTree::countleaf(BiTNode *p){
    if(p){
      m = countleaf(p->lchild);
      n = countleaf(p->rchild);
      if(m + n == 0)
        return 1;
      else
        return m+n；  
    }
    else
      return 0;
  }
  ```
  
  ```c++
  \\拷贝二叉树
  void int BinaryTree::CopyTree(BiTNode *t, BiTNode *s){
     if(s!=NULL) {
       t=new BiTNode;
       t->data=s->data;
       CopyTree(t->lchild,s->lchild);
       CopyTree(t->rchild,s->rchild);
     }
    else
       t=NULL;
  }
  ```
  
  ```c++
  \\先序
  void preorder(*p){
      stack<int> S;
      S.push(*p);
      while(!S.empty()){
          temp = s.pop();
          cout<<temp->data;
          S.push_back(temp->right);
          S.push_back(temp->left);
      }
  }
  ```
  
  ```c++
  \\中序遍历
  void  inorder1(t)
  { 栈S初始化；
      p=t;
      while(p!=NULL || 栈不空）{
        if(p!=NULL) { p进S栈；  
          p=p->lchild; }
        else {  
          退栈到p;
          cout<<p->data;
          p=p->rchild;
        }
      }//while
  }
  ```
  
  ```c++
  \\后序
  void  postorder(t){
      栈S初始化；
      p=t;
      lastNodeVisited=null;
      while(p!=NULL || 栈不空{
          if(p!=NULL) { 
              p进S栈；  
                  p=p->lchild;
          }
          else{ 
              S栈顶元素赋值给temp;
              if(temp.right!=NULL && lastNodeVisited！= temp.right)
                  p=temp->rchild;
              else{
                  cout<<temp->data;
                  S栈退栈到lastNodeVisited；
              }
          }
      }//while
  }
  ```
  
  ### 线索二叉树
  
  若结点有左子树，则其child域指示其左孩子，否则另lchild指向其前驱结点；若结点有右子树，则其rchild指示其右孩子，否则另rchild域指示其后继结点；为了区分结点的指针域是指向孩子还是前驱或后继，在结点中增加两个标志位：ltag和rtag。
  
  #### 森林和二叉树的转换
  
  **森林转换成二叉树：**
  
  1. 将森林中的每棵树转换成相应的二叉树；
  2. 第一棵二叉树不动，从第二棵二叉树开始，依次把后一棵二叉树的根结点作为前一棵二叉树根结点的右孩子，当所有的二叉树连起来之后，此时所得到的二叉树就是由森林转换得到的二叉树。
  
  #### 树和二叉树的转换
  
  **树转换成二叉树：**
  
  1. 加线：将树中所有相邻兄弟之间加一条连线
  2. 抹线：对树中的每个结点，只保留它与第一个孩子结点之间的连线，删除它与其他孩子结点之间的连线
  3. 旋转：以树的根结点为轴心，将整棵树顺时针旋转45，使之成为一棵层次分明的二叉树。
  
  **二叉树转换成树：**
  
  1. 若某结点是双亲的左孩子，则把该结点的右孩子、右孩子的右孩子……都与该结点的双亲结点用线连起来。
  2. 删去原二叉树中所有的双亲结点与其右孩子结点的连线。
  3. 整理由（1）、（2）两步所得到树或森林，使之结构层次分明。
  
  ## 树的遍历
  
  - 先根（先序）
  - 后根（后序）
  
  ## 哈夫曼树
  
  **路径：**从一个祖先结点到子孙结点之间的分支构成这两个结点间的路径
  **路径长度：**路径上的分支数目称为路径长度
  **树的路径长度：**从根到每个结点的路径长度之和
  **结点的带权路径长度：**从根到该结点的路径长度与该结点权的乘积
  **哈夫曼树：**假设有n个权值$(w_1,w_2,...,w_n)$，构造有n个叶子结点的二叉树，每个叶子结点有一个$w_i$作为它的权值，则带权路径长度最小的二叉树称为哈夫曼树
  
  ### 哈夫曼树的构造
  
  1. 根据给定的n个权值 ，构造n棵只有一个根结点的二叉树， n个权值分别是这些二叉树根结点的权。设F是由这n棵二叉树构成的集合
  2. 在F中选取两棵根结点树值最小的树作为左、右子树，构造一颗新的二叉树，置新二叉树根的权值=左、右子树根结点权值之和
  3. 从F中删除这两颗树，并将新树加入F；
  4. 重复 2) 和3)，直到F中只含一颗树为止；
  
  
  # 图
  
  ## 图的定义
  
  **路径**：在图中由顶点v到v‘的顶点序列
  **回路或环**：第一个顶点和最后一个顶点相同的路径
  **简单回路或简单环**：除第一个顶点和最后一个顶点外，其余顶点不重复出现的回路
  **连通**：顶点v至v’之间有路径存在
  
  **生成树**：极小连通子图。包含图的所有n个结点，但只含图的n-1条边。在生成树中添加一条边之后，必定会形成回路或环。
  
  **完全图**：有n(n-1)/2条边的无向图，其中n是结点个数
  **有向完全图**：有n(n-1)条边的有向图
  **邻接点**：无向图结点的度；有向图结点的出度和入度
  
  ## 图的存储结构
  
  * 邻接矩阵和加权邻接矩阵
  * 邻接表
  
  ### 邻接矩阵和加权邻接矩阵
  
  **无权值的有向图的邻接矩阵**
  设有向图有n个结点，则用n行n列矩阵A表示有向图；如果i至j有一条弧，则A[i,j] =1；否则，A[i,j]=0。注意：A[i,i]=0。出度：i行之和；入度：j列之和
  **无权值的无向图的邻接矩阵**
  设无向图有n个结点，则用n行n列的矩阵A表示该无向图；如果i至j有一条边,则A[i,j]=1 ；否则，A[i,j]=0。注意：A[i,i] = 0。i结点的度：i行或i列之和。为对称矩阵。
  **图的加权邻接矩阵**
  设图有n个顶点，则用n行n列的矩阵A表示该图；如果i至j有一条边（弧）且它的权值为w。则 A[i,j]=w ;否则，A[i,j]= ∞ （或其它标志）；
  
  ```c++
  #define MAXINT  (1<<sizeof(int)*8-1)-1
   CreateGraph(a[][VexNuM],e)
   // VexNuM-图的顶点数,e-边（弧）数
     {  for(i=0;i<VexNuM;i++)
          for(j=0;j<VexNuM;j++) a[i][j]= MAXINT;
     for(k=1;k<=e;k++)
      {  cin<<i<<j<<w;//读入顶点号i,j和边上权w
         a[i-1][j-1]=w;
         a[j-1][i-1]=w;//无向图
      }
    }
  ```
  
  ### 邻接表
  
  设图具有n个结点，则用顶点数组表、边（弧）表表示改有向图或无向图
  **数组顶点表中的分量的形式**
  `data`：结点的数据域，保存结点的数据值
  `firstarc`：结点的指针域，给出自该顶点出发的第一条边（弧）的边结点的地址
  **边表中的结点的形式**
  `adjvex`：结点的数据域，保存结点的数据（比如顶点号）。
  `nextvex`：结点的指针域，给出该结点的下一结点（边）的地址。
  `info`：边结点的数据域，保存边的 权值等。如不是网，此部分可省去。
  
  ```c++
  void  Create(Graph &G,int n,int e)
      //adj：邻接表表头数组， n个顶点,e条弧
  {   for(i=0;i<n;i++) {
       G.adj[i].data=i+1;  
       G.adj[i].firstarc=NULL;}
    for(i=1;i<=e;i++)
     { cin>>u>>v;//输入一条弧<u,v>
       p =new ArcNode;
       p->adjvex=v;
       p->nextarc=adj[u-1].firstarc;
       G.adj[u-1].firstarc=p;
     }
  }
  
  typedef struct ArcNode{
      int adjvex;
      ArcNode nextarc;
  }ArcNode;
   typedef struct{
     int data;
    ArcNode *firstarc;}Vexnode;
  Typedef struct{
     Vexnode adj[max];
     int n,e;}Graph;
    
  #define max 20
  typedef struct ArcNode{
      int adjvex;
      ArcNode nextarc;
  }ArcNode;
   typedef struct{
     Telemtype data;
    ArcNode *firstarc;}Vexnode;
  Typedef struct{
     Vexnode adj[max];
     int n,e;}Graph;
  int serach(Graph G,Telemtype e){
       for(int i=0;i<G.n;i++)
          if(G.adj[i].data==e) return i;
            return –1;}
  
  
  void  Create(Graph &G,int n,int e)
      { int h,t;
     for(int i=0;i<n;i++) {
       cin>> G.adj[i].data;  
       G.adj[i].firstarc=NULL;}
    for(int j=1;j<=e;j++)
     { lab:cin>>u>>v;//输入一条弧<u,v>
        h=search(G,u);t=search(G,v);
        if(h==-1||t==-1){ cout<<“无此顶点“<<endl;
                                     goto lab;}
        p =new ArcNode;
        p->adjvex=t;
        p->nextarc=adj[h].firstarc;
        G.adj[h].firstarc=p;}}
  
  ```
  
  ## 图的遍历
  
  * 深度优先（DFS）
    1）访问顶点v
    2）从v的未被访问的邻接点出发，继续对图进行深度优先遍历，若从某点出发所有邻接点都已访问过，退回前一个点继续上述过程，若退回开始点，结束。
  
  * 广度优先（BFS）
    1） 访问顶点v ；
    2）访问同v相邻的所有未被访问的邻接点w1,w2, …wk；
    3）依次从这些邻接点出发，访问它们的所有未被访问的邻接点; 依此类推，直到图中所有访问过的顶点的邻接点都被访问；
  
  ## 生成树和最小生成树
  
  每次遍历一个连通图将图的边分成遍历所经过的边和没有经过的边两部分，将遍历经过的边同图的顶点构成一个子图，该子图称为生成树
  
  ### 最小生成树
  
  **定义**：生成树中边的权值之和最小的树
  **Kruskal算法**
  **Prim算法**
  
  ## 有向无环图及应用
  
  ### 拓扑排序
  
  1. AOV网：用顶点表示活动，边表示活动的优先关系的有向图称为AOV网。AOV网不允许有回路
  2. 拓扑有序序列：把AOV网络中各顶点按照它们相互之间的优先关系排列一个线性序列的过程。若$v_i$是$v_j$前驱，则$v_i$一定在$v_j$之前；对于没有优先关系的点，顺序任意。
  
  (1)在有向图中选一个没有前驱的顶点且输出之
  (2)从图中删除该顶点和所有以它为尾的弧
  (3)重复上述两步，直至全部顶点均已输出；或者当图中不存在无前驱的顶点为止(此时说明图中有环）
  
  ```c++
  //NUM:图顶点数，adj：邻接表表头
  void  topsort(adj)
  { 初始化栈;
     findindegree(adj,indegree);
     for(i=0;i<NUM;i++)
       if(indegree[i]==0)
           push(i);
     count=0;
   while(栈不空）
   { i=pop();
     cout<<i+1;
    count++;
     for(p=adj[i].firstarc;p!=NULL;p=p->next)
      {  k=p->adjvex-1;
         indegree[k]--;
      if(indegree[k]==0)  push(k); }
     }
   if(count<NUM) cout<<“有回路”<<endl;
  }
  ```
  
  ### 关键路径
  
  **AOE网**：带权的有向无环图，其中顶点表示事件，弧表示活动，权表示活动持续时间。在工程上常用来表示工程进度计划。
  
  **源点**：整个工程的开始点（入度为0）
  
  **汇点**：整个工程的结束点（出度为0）
  
  **活动（有向边）**：它的权值定义为活动进行所需要的时间。方向表示事件的优先关系。
  
  **事件的最早发生时间ve(j)**：从源点到j结点的最长路径
  
  **事件的最迟发生时间vl(j)**：不影响工程的如期完工，事件j必须发生的时间
  
  活动$a_i$由弧<j,k>表示，持续时间记为*dut<j,k>*,则有：
  
  * 活动的最早开始时间：e(i)=ve(j)
  * 活动的最迟开始时间：l(i)=vl(k)-dut(<j,k>)
  
  **活动余量**：l(i)-e(i)的差
  
  **关键活动**：活动余量为0的活动
  
  **关键路径**：从源点到汇点的最长的一条路径，或者全部由关键活动构成的路径
  
  $$
  ve(j)=Max\{ve(j)+dut(<i,j>)\}
  $$
  
  $$
  vl(i)=Min\{vl(j)-dut(<i,j>)\}
  $$
  
  ### 最短路径
  
  **Dijkstra算法**
  
  图用邻接矩阵表示，设置集合U与辅助数组dist，
  源点vo图的顶点数为n。
  g[i,j]代表<i,j>上的权；
  <i,j>不存在，g[i,j]就为无穷大。
  (1)  $dist[i]= g[v_0,i]     i=1,2,….,n$
  (2)  $U=[v_0]$;
  (3) 下面步骤重复n-1次
    a) 选择v使得满足   $dist[v]=min{ dist[u]|u\notin U}$
    b) v加到集合U中$U=U\cup {v}$
    c) 修改不在集合U中顶点的dist值$dist[w]=min{dist[w],dist[v]+g[v,w]|w\notin U}$
  
  ```c
  #define NUM          
  void shortpath_dij(g[][NUM],v0) //v0为源点，值为1到NUM
  {  
       for(i=0;i<NUM;i++){
           set[i]=0;
          dist[i]=g[v0-1][i];}
        set[v0-1]=1;
    for(i=1;i<NUM;i++)
      { min=MAXINT;
         for(w=0;w<NUM;w++)
             if(set[w]= =0 && dist[w]<min)
                   {  v=w;  min=dist[w]; }
          set[v]=1;
         for(w=0;w<NUM;w++)
          if(set[w]= =0 && dist[v]+g[v][w]<dist[w])
            dist[w]=dist[v]+g[v][w];
      }//for i
  } //shortpath_dij
  ```
  
  # 查找
  
  ## 静态查找表
  
  ### 顺序表的顺序查找
  
  **应用范围**：顺序表或线形链表表示的表，表内元素之间无序
  
  ```cpp
    search(st,key，n)
  //在有n个数据的表st中找key
    {  i=n-1;
       while(i>=0 && st[i]!=key)
            i- -;
       if(i<0) return -1;
         //查找不成功返回-1
       else return i;
        //查找成功返回下标号
     }
  ```
  
  ### 顺序表的二分查找
  
  查找过程：每次将待查记录所在区间缩小一半
  适用条件：采用顺序存储结构的有序表
  算法实现
  设表长为n，low、high和mid分别指向待查元素所在区间的上界、下界和中点,k为给定的待查值
  初始时，令low=1,high=n,mid=(low+high)/2 向下取整
  让k与mid指向的记录比较：
  
  * 若k=r[mid]，查找成功，结束
  * 若k<r[mid]，则high=mid-1
  * 若k>r[mid]，则low=mid+1
    重复上述操作，直至low>high时，查找失败
  
  ```cpp
  递归：
  bin_search(st[],key,l,h)
     {  
        if(l<=h) {
          mid=(l+h)>>1;
            if( st[mid] = = key)
            return mid;
        else  if(st[mid]>key)
            return  bin_search(st,key,l,h-1);
          else 
          return bin_search(st,key,l+1,h)
      }
     else return -1;
   }
  ```
  
  ### 顺序表的分块查找
  
  数据组织：
  将表分成几块，块内无序，块间有序；先确定待查记录所在块，再在块内查找
  
  * 用数组存放待查记录,
  * 建立索引表，由每块中最大（小）的关键字及所属块位置的信息组成。
  
  ## 动态查找表
  
  ### 二叉排序树和二叉平衡树
  
  #### 二叉排序树
  
  1、定义：二叉排序树（Binary Sort Tree）或者是一棵空树；或者是具有下列性质的二叉树：
  
  * 若左子树不空，则左子树上所有结点的值均小于它的根结点的值；
  * 若右子树不空，则右子树上所有结点的值均大于它的根结点的值；
  * 左、右子树也分别为二叉排序树；
  
  2、查找：
  步骤：若根结点的关键字值等于查找的关键字，成功。
  否则，
  
  * 若小于根结点的关键字值，递归查左子树。
  * 若大于根结点的关键字值，递归查右子树。
  * 若子树为空，查找不成功。
  
  ```cpp
   TreeNode *SearchBST (t,key )
   /*在二叉分类树查找关键字之值为 key 的结点，找到返回该结点的地址，否则返回空。t 为二叉分类树的根结点的指针。*/
   { if(t= =NULL||key= =t->data) return t;
     else if（key<t->data)  
        return SearchBST(t->lchild,key);
         else return SearchBST(t->rchild,key); 
    } // SearchBST
  ```
  
  插入算法：
  首先执行查找算法，找出被插结点的父亲结点。
  判断被插结点是其父亲结点的左、右儿子。将被插结点作为叶子结点插入。
  若二叉树为空。则首先单独生成根结点。
  **注意：新插入的结点总是叶子结点。**
  
  ```cpp
  void InsertBST(t，key)
    //在二叉排序树中插入查找关键字key
    {
       if(t==NULL){
           t=new BiTree; 
           t->lchild=t->rchild=NULL;
           t->data=key;
           return; }
       if(key<t->data ) InsertBST(t->lchild,key);
          else InsertBST (t->rchild, key ); 
    }
  void CreateBiTree(tree,d[ ],n）
  //n个数据在数组d中，tree为二叉排序树根
    {  tree=NULL;
        for(i=0;i<n;i++)
         InsertBST(tree,d[i]);
  }
  ```
  
  二叉排序树中一个结点的删除
  **在二叉排序树中删除结点的原则是：删除结点后仍是二叉排序树。**
  
  设在二叉排序树被删除结点是x，其双亲结点为f。
  情况讨论： 
  
  * x为叶子结点，则直接删除
  * x只有左子树xL或只有右子树xR ,则令xL或xR直接成为双亲结点f的子树； 
  * x即有左子树xL 也有右子树xR：在xL中选值最大的代替x，该数据按二叉排序树的性质应在最右边。
  
  ```cpp
  void  delete(p)
   {
      if(p->rchild = = NULL)
        { q=p;
           p=p->lchild;
           delete q; }
     else  if(p->lchild= =NULL)
       { q=p; p=p->rchild;
          delete q;
       }
      else {
       q=p; s=p->lchild;
     while(s->rchild!=NULL)
       {q=s;  s=s->rchild;}
       p->data=s->data;
       if(q!=p) q->rchild=s->lchild;
        else q->lchild=s->lchild;
    }
           delete s;
  }
  ```
  
  #### 平衡二叉树
  
  **平衡因子（平衡度）**：结点的平衡因子是结点的左子树的高度减去右子树的高度。（或反之定义）
  **平衡二叉树**：每个结点的平衡因子都为 1、－1、0 的二叉树。或者说每个结点的左右子树的高度最多差1的二叉树。
  
  
  如何平衡？
  Adelson解决思路：
  
  * 不涉及到不平衡点的双亲，即以不平衡点为根的子树的高度应保持不变。
  * 新结点插入后，向根回溯找到第一个原平衡因子不为0的结点。新插入的结点和第一个平衡因子不为0的结点之间的结点，其平衡因子原皆为0
  * 在调整中，仅涉及前面所提到的最小子树
  * 仍应保持排序二叉树的性质不变。
  
  平衡种类分析
  **左调整**：
  
  1. LL情况：将B作为子树的根结点，将B的右子树作为A的左子树（A为第一个不平衡的点）
  2. LR情况：以C（B的右结点）为根结点，B为C的左结点，A为C的右结点，将CL作为B的右结点，CR作为A的左结点
     **右调整**：
     左调整的镜像
  
  平衡树建立方法：
  
  1. 按二叉排序树插入结点
  2. 如引起结点平衡因子变为|2|，则确定旋转点，该点是离根最远（或最接近于叶子的点）
  3. 确定平衡类型后进行平衡处理，平衡后以平衡点为根的子树高不变
  
  
  
  **B-树中结点的删除**
  
  删除发生在底层：
  
  1. 被删关键字所在结点中的关键字数目大于等于$\lceil m/2\rceil$，直接删除
  2. 被删除结点中关键字数目$\lceil m/2-1\rceil$，而相邻的左（右）兄弟中数据大于$\lceil m/2-1\rceil$，此时左（右兄弟）中最大（小）的数据上移到双亲中，双亲中接（靠）在它后（前）面的数据移到被删数据的结点中
  3. 其左右兄弟结点中数据都是$\lceil m/2-1\rceil$，此时和左（右）兄弟合 并，合并时连同双亲中相关的关键字。此时，双亲中少了一项，因此又可能引起双亲的合并，最坏一直到根，使B-树降低一层
  
  删除不在最底层：
  
  * 在大于被删除数据中选最小的代替被删数据
  
  
  
  #### B+树
  
  1. 有n棵子树的结点中有n个关键字
  2. 所有叶子结点中包含全部关键字信息及对应记录位置信息
  3. 所有非叶子为索引，只含关键字而且仅有子树中最大或最小的信息
  4. 非叶最底层顺序联结，这样可以顺序查找
  
  ## 哈希查找表
  
  **哈希基本思想**：在记录的存储地址和它的关键字之间建立一个确定的对应关系
  
  **哈希函数**：在记录的关键字与记录的存储位置之间建立的一种对应关系
  
  **冲突**：多个不同的关键字对应的哈希函数值是相同的
  
  ### 常用的哈希函数
  
  1. 直接定址法：取关键字或者关键字的线性函数作为哈希地址
  2. 数字分析法：对关键字进行分析，去关键字的若干位或其组合作哈希地址，适于关键字位数比哈希地址位数大，且可能出现的关键字事先知道
  3. 平方取中法：取关键字平方后中间若干位作哈希地址
  4. 折叠法：将关键字分割成位数相同的几部分，然后取这几部分的叠加和（舍去进位）作哈希地址
  5. 除留余数法：取关键字被某个不大于哈希表表长m的数r除后所得余数作哈希地址，即$H(key)=key \% p$
  6. 随机数法：取关键字的伪随机函数值作哈希地址
  
  ### 冲突解决
  
  #### 开放定址法
  
  当冲突发生时，形成一个探查序列；沿此序列逐个地址探查，直到找到一个空位置（开放的地址），将发生冲突的记录放到该地址中，即$H_i=(H(key)+d_i)\% m,i=1,2,...,k(k<m-1)$
  
  * H(key)：哈希函数
  * m：哈希表表长
  * $d_i$：增量序列
  
  分类：
  
  * 线性探测再散列
  * 二次探测再散列
  * 伪随机探测再散列
  
  #### 链地址法
  
  将所有关键字为同义词的记录存储在一个单链表中，并用一维数组存放头指针
  
  # 内部排序
  
  ## 插入类排序
  
  ### 直接插入排序
  
  方法：数据r[1]到r[i-1]是有序的，r[i]到r[n]是任意次序的。将r[i]插入到r[1]到r[i-1]中并使得r[1]到r[i]有序。i从2开始到结束进行n-1次
  
  ```c++
  void InsertSort(int r[ ],int n)
  {
  	int i,j;
  	int x;
  	for(i=1;i<n;i++)
  	{
  		 x=r[i];
  		for(j=i-1;j>=0;j--)
  			if(r[j]>x) 
  				r[j+1]=r[j];
  			else break;
  		 r[j+1]=x;
  	}
  }
  ```
  
  **折半插入排序**：由于插入第i个元素到r[1]到r[i-1]之间时，前i个数据是有序的，所以可以用折半查找确定插入位置，然后插入。
  
  ```c++
  void BInsertSort (int r[],int n)
  {   int i,low,high,m;
      int x;
       int j;
     for ( i = 1;  i<=n-1 ; ++i )
      {  x= r[i]; low = 0 ; high = i-1 ;
         while ( low <= high )       
            {  m = (low+high)>>1; 
               if  ( x< r[m])  high = m -1 ;
              else  low = m + 1; 
  	   }
         for ( j=i-1; j>=high+1; --j ) r[j+1] = r[j];
  	
         r[high+1] = x;
       }
    }  // BInsertSort
  
  ```
  
  ### shell排序
  
  方法：先取一个正整数$d_1<n$，把所有相隔$d_1$的数据作为一组，组内进行直接插入排序；然后取$d_2<d_1$，重复上述分组和排序操作；直至$d_i=1$。
  
  ```c++
  void ShellSort(int r[],int n)
   { int i,j,d;
       int x;
       d=n/2;
    while(d>=1){
      for(i=d ;i<n;i++)
        {  x=r[i];
           for(j=i-d;j>=0;j-=d)
             if(r[j]>x)   r[j+d]=r[j];
              else     break;
            r[j+d]=x;
        }//for i
         d>>=1;
        }//while
  }//ShellSort
  ```
  
  ## 交换类排序
  
  ### 冒泡排序
  
  **原理**: n个数据，r[1]与r[2]比较，大者在r[2]中，r[2]与比较r[3]，大者在r[3]，依此类推，r[n-1]与r[n]比较，大者在r[n]，这样，一趟结束后，r[1]到r[n]中最大者在r[n]中；第二趟在r[1]到r[n-1]个数据中进行;按此方法，最后一趟在r[1]与r[2]比较，大者在r[2]。这样一共进行n-1趟完成排序。
  
  ```c++
  void bubblesort(int r[],int n){  
      int i,j;
      int x;
      int flag;
      for(i=n-1;i>0;i--){
          flag=1;
          for(j=0;j<i;j++)
              if(r[j]>r[j+1]){ 
                  x=r[j];
                  r[j]=r[j+1];
                  r[j+1]=x;
                  flag=0;
              }
          if(flag) 
              break;
      }
  }
  ```
  
  ### 快速排序
  
  **原理**: 若序列中有n个元素，将第一个数据作为支点，将它放在表中合适的位置。以支点为界，序列分成两部分。其中左边数据小于等于支点，右半部分数据大于等于支点。然后，对左右两部分分别进行递归处理，直至排好序为止。
  
  **将支点放在表合适位置——划分算法**：
  
  设两个指针，i和j，开始分别指向表的开始与结束，任一时刻，支点或在i处（开始在i处），或在j处：
  
  * 支点在i处：若r[i]<=r[j]，两者位置合适，j减1；否则，两者位置不合适，r[i]与r[j]对换，i加1；
  
  * 支点在j处：若r[i]<=r[j]，两者位置合适，i加1；否则，两者位置不合适，r[i]与r[j]对换，j减1；
  * 重复直到i与j相等。
  
  *不适合基本有序的情况*
  
  ## 选择类排序
  
  ### 简单选择排序
  
  **方法**：第一趟在r[1]到r[n]中挑出最小的，放在r[1]；第二趟在r[2]到r[n]中挑出最小的，放在r[2]，依此类推；最后一趟在r[n-1]和r[n]挑出小者，放在r[n-1]，这样，共进行n-1趟。
  
  ```c++
  void selectsort(int a[],int n){
      int i,j,k;
      int x;
      for(i=0;i<n-1;i++){
          k=i;
          for(j=i+1;j<n;j++)
              if(a[j]<a[k])
                 k=j;
          x=a[k];
          a[k]=a[i];
          a[i]=x;
      }
  }
  ```
  
  ### 堆排序
  
  **定义**:n个元素的序列$\{k_1,k_2,...,k_n\}$，当且仅当对任一$k_i$满足以下关系时，称之为堆
  $$
  \left\{
  \begin{aligned}
  k_i \le k_{2i}\\
  k_i \le k_{2i+1}
  \end{aligned}
  \right.
  或
  \left\{
  \begin{aligned}
  k_i \ge k_{2i}\\
  k_i \ge k_{2i+1}
  \end{aligned}
  \right.
  $$
  分别称之为**最大化堆**和**最小化堆**
  
  #### Floyed筛选算法
  
  设数据在r[1]到r[n]中，作为一棵完全二叉树，二叉树的左、右子树都是堆，现在将根考虑进去，保证二叉树仍是堆。
  
  * 完全二叉树只有一个结点(即为叶),不需处理
  * 根的值比两个儿子的值都大(大顶堆,小顶堆反之定义),不需处理
  * 根的数据与值大的儿子值对换,问题转换成对子树的处理问题
  
  **建立初始化堆**:从标号最大的有孩子的点i($\lfloor n/2\rfloor$)开始筛选,然后i减1,直到i为1为止
  
  ```c++
  void sift(r[],k,m){
      i=k;
      j=2*i;
      x=r[i];
      while(j<=m){ 
          if(j<m &&r[j]<r[j+1])
              j++;
          if(x<r[j]){
              r[i]=r[j];
              i=j;
              j<<=1;
          }//j*2
          else
              j=m+1;
      }
      r[i]=x;
  }
  ```
  
  ### 基数排序
  
  一个数据看成有t位组成，$(K_1,K_2, …… K_t)$，作为一个元组，大小按字典序定义 
  
  1. 分配：将右起第 j 位数字相同的数放入同一桶。比如数字为1者， 则放入桶$B_1$。其余类推 …… 
  
  2. 收集：按$B_0、B_1、B_2、  ……  B_9$的顺序进行收集
  3. 重复1，2从最右位直到最左位共t次
  
  ## 总结
  
  |   直接插入排序   | 比较次数            |      移动次数       | 交换次数 |         赋值次数          |
  | :--------------: | ------------------- | :-----------------: | :------: | :-----------------------: |
  | 最好情况（有序） | $n-1$               |          0          |   NULL   |             0             |
  | 最坏情况（逆序） | $\sum_{i=2}^2(i-1)$ | $\sum_{i=2}^2(i-1)$ |   NULL   | $(n-1)+\sum_{i=2}^n(i-1)$ |
  |     平均情况     | $O(n^2)$            |      $O(n^2)$       |   NULL   |         $O(n^2)$          |
  
  |     冒泡排序     | 比较次数            | 移动次数 |       交换次数       |  赋值次数  |
  | :--------------: | ------------------- | :------: | :------------------: | :--------: |
  | 最好情况（有序） | $n-1$               |   NULL   |          0           |     0      |
  | 最坏情况（逆序） | $(n-1)+(n-2)+...+1$ |   NULL   | $（n-1)+(n-2)+...+1$ | 3*交换次数 |
  |     平均情况     | $O(n^2)$            | $O(n^2)$ |         NULL         |  $O(n^2)$  |
  
  |   简单选择排序   | 比较次数            | 移动次数 | 交换次数 |  赋值次数  |
  | :--------------: | ------------------- | :------: | :------: | :--------: |
  | 最好情况（有序） | $(n-1)+(n-2)+...+1$ |   NULL   |    0     |     0      |
  | 最坏情况（逆序） | $(n-1)+(n-2)+...+1$ |   NULL   |  $n-1$   | 3*交换次数 |
  |     平均情况     | $(n-1)+(n-2)+...+1$ |   NULL   |  $O(n)$  |  $O(n^2)$  |
  
  **时间复杂度为$O(nlogn)$**：快速排序、堆排序、归并排序
  
  **时间复杂度为$O(n^2)$**：直接插入排序、冒泡排序、简单选择排序
  
  * 所有的简单排序方法（包括：直接插入、冒泡和选择）和堆排序的空间复杂度为$O(1)$
  * 快速排序空间复杂度为$O(logn)$，为递归程序执行过程中，栈所需的辅助空间为$O(logn)$或$O(n)$
  * 归并排序空间复杂度为$O(n)$
  
  **不稳定的排序方法**：选择排序、快速排序、堆排序、希尔排序
  
  # 外部排序
  
  **生成合并段：**
  
  1. 读入文件的部分记录到内存
  2. 在内存中进行内部排序
  3. 将排好序的这些记录写入外存，形成合并段
  4. 再读入该文件的下面的记录，往复进行，直至文件中的记录全部形成合并段为止
  
  **外部合并：**将上一阶段生成的合并段调入内存，进行合并，直至最后形成一个有序的文件
