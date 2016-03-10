---
title: IOS之表视图TableView的使用
date: '2016-03-10'
description:
categories:

tags:ios

---

>

### 调节 UITableView 的 TOP 

>

***IOS 状态栏高度 20***

>

<img src="{{urls.media}}/IOS之表示图的使用/2.jpg" alt="" width="500" height="500">

>

---

>

### iOS TableViewController 使用

>

<img src="{{urls.media}}/IOS之表示图的使用/1.png" alt="" width="500" height="600">

>

* 新建一个 TableViewController
* 新建一个 Cocoa Touch Class（Main.storyboard）
* 向 Main.storyboard 中拖入 Table View Controller
* 将 Table View Cell 的 identifier 设置为 reuseIdentifier
* 通过 reuseIdentifier 获取 UITableViewCell
* 通过 Tag 从 UITableViewCell 中获取 UILabel

>

    class TableViewController: UITableViewController {
        
        // Tag
        var LABEL_CELL_TAG = 1
        
        override func viewDidLoad() {
            super.viewDidLoad()
            
            // Uncomment the following line to preserve selection between presentations
            // self.clearsSelectionOnViewWillAppear = false
            
            // Uncomment the following line to display an Edit button in the navigation bar for this view controller.
            // self.navigationItem.rightBarButtonItem = self.editButtonItem()
        }
        
        override func didReceiveMemoryWarning() {
            super.didReceiveMemoryWarning()
            // Dispose of any resources that can be recreated.
        }
        
        // MARK: - Table view data source
        
        // 告知视图，有多少个section需要加载到table里
        override func numberOfSectionsInTableView(tableView: UITableView) -> Int {
            // #warning Incomplete implementation, return the number of sections
            return 1
        }
        
        // 告知controller每个section需要加载多少个单元或多少行
        override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
            // #warning Incomplete implementation, return the number of rows
            return 3
        }
        
        // 返回UITableViewCell的实例，用于构成table view.这个函数是一定要实现的
        override func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
            // 通过Identifier获取UITableView
            let cell = tableView.dequeueReusableCellWithIdentifier("reuseIdentifier", forIndexPath: indexPath)
            // 通过Tag获取对象UILabel
            let label = cell.viewWithTag(LABEL_CELL_TAG) as! UILabel
            label.text = "Hello World"
            // Configure the cell...
            return cell
        }
    }
    
>

---

>