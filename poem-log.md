---
title: poem日志
date: 2018-06-21 15:38:15
tags: 游戏开发
categories: 游戏开发
---
## 2018/6/14
>取消“剧情模式”中的“好感度”和“章节奖励”

```
(PlotSelectPanel.as:76)
//临时取消章节奖励按钮 -- wcj
//btnAward.clickHandler = Handler.create(this,onShowAwardFun,null,false);
btnAward.visible = false;		
(btnSp._childs[3] as Image).visible = false;
btnSp._childs[4]._y = 198;

(PlotSelectPanel.as:117)
//临时取消好感度 -- wcj
loveBg.visible = false;```


## 2018/6/19
>交叉营销，更换“更多好玩”图片

首先做判断只有在小游戏环境下才显示，然后实例化 `RecommendUI`，设置大小位置，以及加点击事件。

```
if(Browser.onMiniGame)
{
	/**实例化RecommendUI**/
	var t: RecommendUI = new RecommendUI({appid: "wx1b18ebf8b4a292fb"});
	var mgHandler:Handler;
	Laya.stage.addChild(t);
	btnMoreGames.name = "btnMoreGames";
	btnMoreGames.x = 0;
	btnMoreGames.bottom = 106;
	btnMoreGames.width = 101;
	btnMoreGames.height = 114;
	btnMoreGames.skin = "res/share/btnMoreGames.png";
	btnMoreGames.on(Event.CLICK,this,showGames,[t]);
	_funcMainLayer.addChildAt(btnMoreGames,_funcMainLayer.numChildren-1);
}```

在（FunctionMainSp.as:181）判断在主界面正常显示的时候才显示“更多好玩”按钮。

```
/***显示face****/
public function showFace(flag:int):void
{
	switch(flag)
	{
		case 1://正常主界面显示
			{
				_buttonContainersObj[FACE_BUTTON_POSITION_TOP].visible = false;
				_buttonContainersObj[FACE_BUTTON_POSITION_MODE].visible = true;
				_buttonContainersObj[FACE_BUTTON_POSITION_BOTTOM].visible = true;
				if(Browser.onMiniGame)
				{
					(this.getChildByName("btnMoreGames") as Image).visible = true;
				}
				(this.getChildByName("_logo") as Image).visible = true;
					break;
				}
		case 2://隐藏活动按钮和模式按钮
		{
			_buttonContainersObj[FACE_BUTTON_POSITION_TOP].visible = false;
			_buttonContainersObj[FACE_BUTTON_POSITION_MODE].visible = false;
			_buttonContainersObj[FACE_BUTTON_POSITION_BOTTOM].visible = true;
			if(Browser.onMiniGame)
			{
				(this.getChildByName("btnMoreGames") as Image).visible = false;
			}
			(this.getChildByName("_logo") as Image).visible = false;
			break;
		}
		default:
		{
			break;
		}
	}
}```


> 小游戏包大小限制删掉图片 lihui01.png 

Game.as:198 注释掉
LoginManager.as:46 注释掉



## 2018/6/21
>增加“关于”界面

PlayerInfoPanel.as：添加“关于”按钮

```
(PlayerInfoPanel.as:26)
import util.ButtonUtil;

(PlayerInfoPanel.as:32)
import view.panel.pop.ConfirmPanel;

(PlayerInfoPanel.as:74)
/**关于按钮**/
private var btnAbout: Button;

(PlayerInfoPanel.as:125)
/**关于按钮 - wcj**/
btnAbout = ButtonUtil.commonBtn("common/button_green1.png","关 于",this);
btnAbout.x = 376;
btnAbout.y = 666;
btnAbout.width = 160;
btnAbout.height = 54;
btnAbout.labelSize = 26;	
btnAbout.labelPadding = "10,0,14,0";
btnAbout.on(Event.CLICK, this, onClickAbout);

(PlayerInfoPanel.as:301)
/**点击关于按钮回调**/
private function onClickAbout(): void
{
	PanelManager.openPanel(PanelManager.CONFIRM_PANEL,"txtTitle","txtContent");
}```


ConfirmPanel.as：点击阴影关闭面板

```
(ConfirmPanel.as:38)
this.clickShadowHandler = Handler.create(this,onCloseThis,null,false);

(ConfirmPanel.as:66)
private function onCloseThis():void
{
	PanelManager.closePanel(this.panelId);
}```


PanelManager.as：关于面板样式自定义

```
(PanelManager.as:288)
/**关于面板**/
if(pid == 10011)
{
	panel["btnConfirm"].width = 160;
	panel["btnConfirm"].height = 54;
	panel["btnConfirm"].x = (panel["bg"].width - panel["btnConfirm"].width)*0.5;
	panel["btnConfirm"].bottom = 42;
	panel["btnConfirm"].label = "确 定";
	panel["btnConfirm"].labelSize = 26;
	panel["btnConfirm"].labelPadding = "10,0,14,0";	
}```

