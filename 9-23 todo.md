## 面经补充

1. **虚拟地址是什么，怎么转换成物理地址的，页表的构成，MMU**

2. DNS怎么将域名解析为IP地址的；

3. 操作系统中的锁和原子操作；

4. 共享内存，一个进程怎么知道另一个进程写入了，怎么确定共享内存的物理地址的；

5. https加密的过程：

   1. https：四次加密过程
   2. 讲一下ssl/tls加密流程
   3. ssl/tls: 摘要算法，数字签名，数字证书
   4. 

6.  二叉树层序遍历构建一棵树

   ```
   #include <iostream>
   #include <vector>
   #include <queue>
   #include <stdexcept>
   
   using namespace std;
   
   // 二叉树节点定义
   struct TreeNode {
       int val;
       TreeNode *left;
       TreeNode *right;
       TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
   };
   
   /**
    * 根据层序遍历结果构建二叉树
    * @param levelOrder 层序遍历数组，-1 表示空节点
    * @return 构建好的二叉树的根节点
    */
   TreeNode* buildTreeFromLevelOrder(const vector<int>& levelOrder) {
       // 处理空输入
       if (levelOrder.empty() || levelOrder[0] == -1) {
           return nullptr;
       }
   
       // 创建根节点
       TreeNode* root = new TreeNode(levelOrder[0]);
       queue<TreeNode*> q; // 用于存储当前层的节点，辅助构建下一层
       q.push(root);
   
       int index = 1; // 从数组的第二个元素开始处理
   
       while (!q.empty() && index < levelOrder.size()) {
           TreeNode* current = q.front();
           q.pop();
   
           // 处理左孩子
           if (levelOrder[index] != -1) {
               current->left = new TreeNode(levelOrder[index]);
               q.push(current->left); // 将左孩子加入队列，用于后续处理其子女
           }
           index++;
   
           // 处理右孩子（注意判断是否超出数组范围）
           if (index < levelOrder.size() && levelOrder[index] != -1) {
               current->right = new TreeNode(levelOrder[index]);
               q.push(current->right); // 将右孩子加入队列
           }
           index++;
       }
   
       return root;
   }
   
   // 辅助函数：层序遍历打印二叉树（验证构建结果）
   void printLevelOrder(TreeNode* root) {
       if (!root) {
           cout << "[]" << endl;
           return;
       }
   
       queue<TreeNode*> q;
       q.push(root);
       vector<int> result;
   
       while (!q.empty()) {
           TreeNode* node = q.front();
           q.pop();
   
           if (node) {
               result.push_back(node->val);
               q.push(node->left);
               q.push(node->right);
           } else {
               result.push_back(-1); // 用 -1 表示空节点
           }
       }
   
       // 移除末尾多余的 -1（优化打印效果）
       while (!result.empty() && result.back() == -1) {
           result.pop_back();
       }
   
       cout << "[";
       for (size_t i = 0; i < result.size(); ++i) {
           if (i > 0) cout << ", ";
           cout << result[i];
       }
       cout << "]" << endl;
   }
   
   // 辅助函数：释放二叉树内存（避免内存泄漏）
   void deleteTree(TreeNode* root) {
       if (!root) return;
       deleteTree(root->left);
       deleteTree(root->right);
       delete root;
   }
   
   // 测试示例
   int main() {
       // 测试用例1：正常二叉树
       vector<int> levelOrder1 = {3, 9, 20, -1, -1, 15, 7};
       TreeNode* root1 = buildTreeFromLevelOrder(levelOrder1);
       cout << "构建结果1: ";
       printLevelOrder(root1); // 应输出 [3, 9, 20, -1, -1, 15, 7]
   
       // 测试用例2：空树
       vector<int> levelOrder2 = {-1};
       TreeNode* root2 = buildTreeFromLevelOrder(levelOrder2);
       cout << "构建结果2: ";
       printLevelOrder(root2); // 应输出 []
   
       // 测试用例3：只有根节点
       vector<int> levelOrder3 = {5};
       TreeNode* root3 = buildTreeFromLevelOrder(levelOrder3);
       cout << "构建结果3: ";
       printLevelOrder(root3); // 应输出 [5]
   
       // 释放内存
       deleteTree(root1);
       deleteTree(root2);
       deleteTree(root3);
   
       return 0;
   }
       
   ```

   ```
   #include <vector>
   #include <iostream>
   #include <queue>
   using namespace std;
   
   struct TreeNode{
       TreeNode* left;
       TreeNode* right;
       int val;
       TreeNode(int v): val(v), left(nullptr), right(nullptr){
   
       }
   };
   
   /***
    *
    * @param result 层序遍历结构
    * @return 二叉树root node
    */
   TreeNode* solution(const vector<vector<int>> result){
       if(result.empty()) return nullptr;
   
       vector<TreeNode*> lastFloor; // 上一层的数据
       auto root = new TreeNode(result[0][0]);
       lastFloor.push_back(root);
       int floorIdx =1;
       while(floorIdx < result.size()){
           auto& floorData = result[floorIdx];
           auto item = floorData.begin();
           vector<TreeNode*> thisFloor; // 上一层的数据
           while(!lastFloor.empty()){
               auto& node = lastFloor.front();
               if(item== floorData.end()) break;
               node->left = new TreeNode(*item);
               thisFloor.push_back(node->left);
               item++;
               if(item== floorData.end()) break;
               node->right = new TreeNode(*item);
               thisFloor.push_back(node->right);
               item++;
           }
           lastFloor = std::move(thisFloor);
           floorIdx++;
       }
       return root;
   }
   
   int main(){
       auto root = solution({{1},{2,3},{4,9},{5,6}});
       return 0;
   }
   ```

   

7. 动态库和静态库，动态库怎么加载进内存的；mmap

8. TCP重传怎么判定，如何加速超时重传；

9. 内存分区，为什么需要内存对齐；

10. cpp新特性补充：future/promise/async

11. 「*TLS False Start*」，跟「*TCP Fast Open*」

