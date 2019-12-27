## 索引节点对象相关内容
### 索引节点对象
```C
struct inode {
    struct hlist_node i_hash;  // 散列表
    struct list_head i_list; // 索引节点链表
    struct list_head i_sb_list;  // 超级块链表
    struct list_head i_dentry;  // 目录项链表
    unsigned long i_ino;  // 节点号
    atmoic_t i_count;  // 引用计数
};
```