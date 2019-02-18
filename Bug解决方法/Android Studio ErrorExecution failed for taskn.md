### Error 
	Error:Execution failed for task ':app:preDebugAndroidTestBuild'. > 
	Conflict with dependency 'com.android.support:support-annotations' in project ':app'. 
	Resolved versions for app (26.1.0) and test app (27.1.1) differ. 
	See https://d.android.com/r/tools/test-apk-dependency-conflicts.html for details.
	
### 问题说明
因为使用的依赖包版本不同的原因，所以，我们强制使用一样的版本即可解决问题

在adroid结点下添加下述代码

	configurations.all {
        resolutionStrategy.force 'com.android.support:support-annotations:26.1.0'
    }

把版本号修改一下即可

![](https://img2018.cnblogs.com/blog/1210268/201901/1210268-20190128214734652-1646193089.png)

### 一劳永逸的办法
上面的办法在当前的项目是已经解决了的，但是，新建一个项目又会出现同样的问题，这就很烦了。

**我们直接通过修改新建一个项目的模板，直接把默认的那些设置改了，即可达成一劳永逸**

我的版本是Android Studio 3.0.1 网上查找修改这些默认设置的时候，资料发现不太一样，自己摸索也是找到了关键的地方

找到路径`Android Studio的根目录\plugins\android\lib\templates\gradle-projects\NewAndroidModule\root`的`shared_macros.ftl`文件，上面自己需要的代码复制在android结点下即可

### 扩展，修改buildToolVersion targetVersion gradleVersion等默认版本  

- appcompat版本号
--
`Android Studio的根目录\plugins\android\lib\templates\gradle-projects\NewAndroidModule`的 `recipe.xml.ftl`

	<#if backwardsCompatibility!true>
	    <dependency mavenUrl="com.android.support:appcompat-v7:25.3.1" />
	</#if>

- compileSdkVersion,buildToolsVersion,targetSdkVersion等版本号
--
`Android Studio的根目录\plugins\android\lib\templates\gradle-projects\NewAndroidModule\root`的`shared_macros.ftl`

		android {
		    compileSdkVersion 25
		    <#if compareVersions(gradlePluginVersion, '3.0.0') lt 0>buildToolsVersion 27.0.1</#if>
		
		    <#if isBaseFeature>
		    baseFeature true
		    </#if>
		
		    defaultConfig {
		    <#if hasApplicationId>
		        applicationId "${applicationId}"
		    </#if>
		        minSdkVersion <#if minApi?matches("^\\d+$")>${minApi}<#else>'${minApi}'</#if>
		        targetSdkVersion 25
		        versionCode 1
		        versionName "1.0"

### 小工具（懒人必备）
[AlterASDefaultSetting](https://github.com/Stars-One/AlterASDefaultSetting)