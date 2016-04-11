---
title: IOS之警告框UIAlertController
date: '2016-03-09'
description:
categories:

tags:ios

---

>

### 警告框

>

***IOS UIAlertController在IOS 9.0后代替了UIAlertView 弹框和UIActionSheet下弹框***

>

<img src="{{urls.media}}/IOS之警告框UIAlertController/1.png" alt="" width="700" height="300">

>

---

>

***Alert***

>

	// UIAlertController
    var alert1: UIAlertController!
    
    // 定义 cancel 按钮 UIAlertAction
    var cancelAction = UIAlertAction(
    	title: "cancel", 
    	style: UIAlertActionStyle.Cancel, 
    	handler: nil)
    	
    // 定义 ok 按钮 UIAlertAction
    var okAction = UIAlertAction(
    	title: "ok", 
    	style: UIAlertActionStyle.Default)
    	{
        (action: UIAlertAction!) -> Void in
        	print("you choose ok")
    	}
    	
    // 定义 save 按钮 UIAlertAction
    var saveAction = UIAlertAction(
    	title: "save", 
    	style: UIAlertActionStyle.Default)
    	{
        (action: UIAlertAction!) -> Void in
        	print("you choose save")
    	}
    	
    // 定义 delete 按钮 UIAlertAction
    var deleteAction = UIAlertAction(
    	title: "delete", 
    	style: UIAlertActionStyle.Destructive)
    	{
        (action: UIAlertAction!) -> Void in
        	print("you choose delete")
    	}
    	
    // 定义 reset 按钮 UIAlertAction
    var resetAction = UIAlertAction(
    	title: "reset", 
    	style: UIAlertActionStyle.Destructive)
    	{
        (action: UIAlertAction!) -> Void in
        	print("you choose reset")
    	}

    @IBAction func onClick(sender: AnyObject) {
    	 // 显示 Alert  
        self.presentViewController(alert1, animated: true, completion: nil)
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 初始化 Alert
        alert1 = UIAlertController(
        	title: "simple alert",
        	message: "this is a simple alert", 
        	preferredStyle: UIAlertControllerStyle.Alert)
        // 添加 UIAlertAction 到 UIAlertController
        alert1.addAction(cancelAction)
        alert1.addAction(resetAction)
        alert1.addAction(okAction)
        
        // Do any additional setup after loading the view, typically from a nib.
    }
	   
>

---

>

***带文本的Alert***

>

    var alert: UIAlertController!
    
    @IBAction func onClick(sender: AnyObject) {
        self.presentViewController(alert, animated: true, completion: nil)
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 初始化带文本框的 Alert
        alert = UIAlertController(title: "login alert", message: "please enter your name and password", preferredStyle: UIAlertControllerStyle.Alert)
        alert.addTextFieldWithConfigurationHandler {
            (textField: UITextField!) -> Void in
            textField.placeholder = "name"
        }
        alert.addTextFieldWithConfigurationHandler {
            (textField: UITextField!) -> Void in
            textField.placeholder = "password"
            textField.secureTextEntry = true
        }
        let loginAction = UIAlertAction(title: "login", style: UIAlertActionStyle.Default) {
            (action: UIAlertAction!) -> Void in
            let name = self.alert.textFields!.first! as UITextField
            let password = self.alert.textFields!.last! as UITextField
            print("name : \(name.text); password : \(password.text)")
        }
        alert.addAction(loginAction)
        

        // Do any additional setup after loading the view, typically from a nib.
    }

>

---

>

***上拉菜单的Alert***

>

    var alert: UIAlertController!
    
    @IBAction func onClick(sender: AnyObject) {
        self.presentViewController(alert, animated: true, completion: nil)
    }
    
    // 定义 cancel、ok、save、delete、reset 的 UIAlertAction
    var cancelAction = UIAlertAction(title: "cancel", style: UIAlertActionStyle.Cancel, handler: nil)
    var okAction = UIAlertAction(title: "ok", style: UIAlertActionStyle.Default){
        (action: UIAlertAction!) -> Void in
        print("you choose ok")
    }
    var saveAction = UIAlertAction(title: "save", style: UIAlertActionStyle.Default){
        (action: UIAlertAction!) -> Void in
        print("you choose save")
    }
    var deleteAction = UIAlertAction(title: "delete", style: UIAlertActionStyle.Destructive){
        (action: UIAlertAction!) -> Void in
        print("you choose delete")
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 初始化上拉菜单
        alert = UIAlertController(title: "simple action sheet", message: "action sheet message", preferredStyle: UIAlertControllerStyle.ActionSheet)
        alert.addAction(cancelAction)
        alert.addAction(deleteAction)
        alert.addAction(saveAction)
        

        // Do any additional setup after loading the view, typically from a nib.
    }

>

---

>

***UIAlertView is deprecatedw***

>

	class ViewController: UIViewController, UIAlertViewDelegate {
	    
	    @IBAction func onClick(sender: AnyObject) {
	        let alertView:UIAlertView = UIAlertView(
	            title:"Alert",
	            message:"Alert tet goes here",
	            delegate: self,
	            cancelButtonTitle: "No",
	            otherButtonTitles: "Yes")
	        alertView.show()
	    }
	    
	    func alertView(alertView: UIAlertView, clickedButtonAtIndex buttonIndex: Int) {
	        NSLog("bottonIndex: %i", buttonIndex)
	    }
	  
	}
	
>