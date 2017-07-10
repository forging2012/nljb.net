---
title: Dialog QML Type
date: '2017-07-10'
description:
categories:

tags:qt

---

>

# Dialog QML Type

>

---

> import QtQuick.Dialogs 1.2

>

	// 基本的 Dialog
    Rectangle {
        color: "red"
        anchors.fill: parent
        MouseArea {
            anchors.fill: parent
            onClicked: {
                dialog.visible = true
            }
        }
    }

    Dialog {
        id: dialog
        visible: false
        title: "Blue sky dialog"

        contentItem: Rectangle {
            color: "lightskyblue"
            implicitWidth: 400
            implicitHeight: 100
            Text {
                text: "Hello blue sky!"
                color: "navy"
                anchors.centerIn: parent
            }
        }
    }
	
>

	// 在 Dialog 中增加对象（日历、按钮）
    Rectangle {
        color: "red"
        anchors.fill: parent
        MouseArea {
            anchors.fill: parent
            onClicked: {
                dateDialog.visible = true
            }
        }
    }

    Dialog {
        id: dateDialog
        visible: false
        title: "Choose a date"
        // 标准按钮
        standardButtons: StandardButton.Save | StandardButton.Cancel | StandardButton.Help

        onClickedButtonChanged: {
            if (standardButtons & StandardButton.Help) {
                console.log("haha")
            }
        }

        onAccepted: {
            console.log("Saving the date " +
            calendar.selectedDate.toLocaleDateString())
        }

        // 日历
        Calendar {
            id: calendar
            // click -> StandardButton.Save
            onDoubleClicked: dateDialog.click(StandardButton.Save)
        }
    }
	
>

	// 消息窗口
    Rectangle {
        color: "red"
        anchors.fill: parent
        MouseArea {
            anchors.fill: parent
            onClicked: {
                messageDialog.visible = true
            }
        }
    }


    MessageDialog {
        id: messageDialog
        title: "May I have your attention please"
        text: "It's so cool that you are using Qt Quick."
        onAccepted: {
            console.log("And of course you could only agree.")
            Qt.quit()
        }
        Component.onCompleted: visible = false
    }
	
>

	MessageDialog {
		title: "Overwrite?"
		icon: StandardIcon.Question
		text: "file.txt already exists.  Replace?"
		detailedText: "To replace a file means that its existing contents will be lost. " +
			"The file that you are copying now will be copied over it instead."
		standardButtons: StandardButton.Yes | StandardButton.YesToAll |
			StandardButton.No | StandardButton.NoToAll | StandardButton.Abort
		Component.onCompleted: visible = true
		onYes: console.log("copied")
		onNo: console.log("didn't copy")
		onRejected: console.log("aborted")
	}
	
>

