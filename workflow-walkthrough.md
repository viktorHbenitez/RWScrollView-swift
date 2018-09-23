# Autolayout for ScrollView + Keyboard Handling in iOS

## autolayout with scroll view and handle keyboard event 

![imagen](../master/assets/problem.gif) 

### KEYBOARD NOTIFICATION:
**Register for notification:**  
UIKeyboardWillShowNotification  
UIKeyboardWillHideNotification  
_you can know its hegiht information about the keyboard animation parameters in case you need to know_  

**Notification userInfo:**  
UIKeyboardFrameBeginUserInfoKey  
UIkeyboardAnimationDurationUserInfoKey  
UIKeyboardAnimationCurveUserInfoKey  

## Steps:

1. Add Observers  
2. Invoke the method to adjust the keyboard insets  
3. Get the user info from the notification  
4. Inset keyboard height frame or remove the insets  
5. Add padding to the top inset scrollview  
6. Remove Observers


```swift
    // 1. Add Observers
    NotificationCenter.default.addObserver(self, selector: #selector(keyboardWillShow(notification:)), name: .UIKeyboardWillShow, object: nil)
    NotificationCenter.default.addObserver(self, selector: #selector(keyboardWillHide(notification:)), name: .UIKeyboardWillHide, object: nil)

    // 6. Remove observers
    deinit {
        NotificationCenter.default.removeObserver(self)
    }
    
    // 2. Invoke the method to adjust the keyboard insets
    @objc func keyboardWillShow(notification: NSNotification) {
        adjustInsetForKeyboardShow(show: true, notification: notification)
    }
    
    @objc func keyboardWillHide(notification: NSNotification) {
        adjustInsetForKeyboardShow(show: false, notification: notification)
    }
    
    
    func adjustInsetForKeyboardShow(show: Bool, notification: NSNotification) {

        // Contains the height of the keyboad frame
        let userInfo = notification.userInfo ?? [:]  // 3. get the user info from the notification
        let keyboardFrame = (userInfo[UIKeyboardFrameBeginUserInfoKey] as! NSValue).cgRectValue
        
        // 4. inset keyboard height frame or remove the insets
        let adjustment = (keyboardFrame.height * (show ? 1 : -1)) 
        
        // 5. Add padding to the top inset scrollview
        fgScrollView.contentInset.bottom += adjustment
        fgScrollView.scrollIndicatorInsets.bottom += adjustment
    }

```


![imagen](../master/assets/solucion.gif)   


