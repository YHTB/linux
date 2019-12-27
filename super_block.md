## 超级块的相关内容

### 超接块对象
```C 
struct super_block {
    struct list_head s_list; // 指向所有的超级块的链表
    dev_t   s_dev;  // 设备标识符
    unsigned long s_blocksize;  // 字节为单位的块大小
    unsigned char s_blocksize_bits;  // 位为单位的块大小
    unsigned char s_dirt;  // 修改标志
    unsigned long long s_maxbytes;  // 文件大小上限
    struct file_system_type s_type;  // 文件系统类型
    struct super_operations s_op;  // 超级块方法
    struct dpuot_operations *dq_op;  // 磁盘限额方法
    struct quotactl_opa *s_qcop;  // 限频控制方法
    struct export_operations *s_export_op;  // 导出方法
    unsigned long s_flags;  // 挂载标志
    unsigned long s_magics;  // 文件系统的幻数
    struct dentry  *s_root;  // 目录挂载点
    struct rw_semaphore s_umount;  // 卸载信号量
    struct semaphore s_lock;  // 超级块信号量
    int s_count;  // 超级块引用计数
    int s_need_sync;  // 尚未同步标志
    atomic_t s_active;  // 活动引用计数
    void *s_security;  // 安全模块
    struct xattr_handler **s_xattr; // 拓展的属性操作
    struct list_head s_inodes;  // inodes链表
    struct list_head s_dirty;  // 修改数据的链表
    struct list_head s_io;  // 回写链表
    struct list_head s_more_io; // 更多的回写链表
    struct hlist_head s_anon;  // 匿名的目录项
    struct list_head s_files;  // 被分配的文件链表
    struct list_head s_dentry_lru;  // 未被使用的目录链表
    int s_nr_dentry_unused;  // 链表中的目录项的数目
    struct block_device *s_bdev;  // 相关的块设备
    struct mtd_info *s_mtd;  // 储存磁盘信息 
    struct list_head s_instances;  // 该类型的文件系统
    struct quota_info s_dquot;  // 限频相关选项
    int s_frozen;  // frozen 标志位
    wait_queue_head_t s_wait_unfrozen;  // 冻结的等待队列
    char s_id[32];  // 文本名字
    void *s_fs_info;  // 文件系统的特殊信息
    fmode_t s_mode;  // 安装权限
    struct semaphore s_vfs_rename_sem;  // 重命名信号量
    u32 s_time_gran;  // 时间截粒度
    char *s_subtype;  // 子类型名称
    char *s_options;  // 已经安装选项
};
``` 

### 超级块操作对象
```C
struct super_operations {
    struct inode *(*alloc_inode)(struct super_block *sb);
    void (*destory_inode)(struct inode*);
    void (*dirty_inode)(struct inode *);
    int (*write_inode)(struct inode* ,int);
    void (*drop_inode)(struct inode*);
    void (*delete_inode)(struct inode*);
    void (*put_super)(struct super_block*);
    void (*write_super)(struct super_block*);
    int (*sync_fs)(struct super_block * sb, int wait);
    int (*freeze_fs)(struct super_block*);
    int (*unfreeze_fs)(struct super_block*);
    int (*statfs)(struct dentry*, struct kstatfs*);
    int (*remount_fs)(struct super_block* ,int * , char * );
    void (*clear_inode)(struct inode*);
    void (*unmount_begin)(struct super_block*);
    int (*show_stats)(struct seq_file*,struct vfamount*);
    int (*show_stats)(struct seq_file*, struct vfamount *);
    ssize_t (*quota_read)(struct super_block * , int , char * , size_t, loff_t);
    ssize_t (quota_write)(struct super_block * , int , const char *, size_t, loff_t);
    int (*bdev_try_to_free_page)(struct super_block*, struct page*, gfp_t);
};
``` 