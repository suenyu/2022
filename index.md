2022年1月14日，运行命令：
```powershell
$GroupName = "Group Creators"
$AllowGroupCreation = $False

Connect-AzureAD

$settingsObjectID = (Get-AzureADDirectorySetting | Where-object -Property Displayname -Value "Group.Unified" -EQ).id
if(!$settingsObjectID)
{
    $template = Get-AzureADDirectorySettingTemplate | Where-object {$_.displayname -eq "group.unified"}
    $settingsCopy = $template.CreateDirectorySetting()
    New-AzureADDirectorySetting -DirectorySetting $settingsCopy
    $settingsObjectID = (Get-AzureADDirectorySetting | Where-object -Property Displayname -Value "Group.Unified" -EQ).id
}

$settingsCopy = Get-AzureADDirectorySetting -Id $settingsObjectID
$settingsCopy["EnableGroupCreation"] = $AllowGroupCreation

if($GroupName)
{
  $settingsCopy["GroupCreationAllowedGroupId"] = (Get-AzureADGroup -SearchString $GroupName).objectid
} else {
$settingsCopy["GroupCreationAllowedGroupId"] = $GroupName
}
Set-AzureADDirectorySetting -Id $settingsObjectID -DirectorySetting $settingsCopy

(Get-AzureADDirectorySetting -Id $settingsObjectID).Values
```
Group Creators内，保留管理员一人。
同时，yammer开启Native Mode。
​

这意味着一些事情，最核心的是：**yammer内不再允许新建群组**。
一个时代，落幕。
​

直接原因是，IT部门和校园管理中心突然微信：
“学校领导需要yammer这个期间的安全管理情况说明，有什么风险，如何管理，是否可以临时关闭”
“孙老师好，在1月17日到2月20期间，在您管理的yammer、语雀、银杏时报的调查问卷，您提供一下安全管理措施，有什么风险没有，如有风险有什么应急预案？您发给我”
​

根本原因是，都是各路神仙，要说明，要预案，要不出问题，要禁访问国外网站权利……所以，有了上述命令的运行。
说到底，不做事，自然能没问题。做自然就有隐患风险。
貌似哲学，无非实际。
​

帝都，要求其实年年事事都有，以往是王铮抗着，事情我做，风险隐患都在。
现在上下一心避免隐患，都可以理解，都是人之常情。
​

常情不是理想，但世间多的是常人，你我都是。
