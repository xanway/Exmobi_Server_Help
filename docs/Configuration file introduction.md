### 2.2 配置文件介绍

 服务参数配置文件（config.xml）ExMobi提供的统一格式的配置文件，开发者在该文件中定义具体业务依赖的配置项，exmobi-business.jar提供了ParamConfig类及参数读取api，支持服务读取相关配置项。config.xml文件中定义的配置信息，会自动在ExMobi管理中展示，并且支持动态修改配置值。 

 config.xml是服务与ExMobi管理端之间的桥梁，通过定义config.xml文件，可以做到参数定义统一配置，简化服务部署。 

config.xml文件示例如下： 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<config>
    <group id="self" name="自定义参数配置">
		<paralist>
			<param>
				<paraKey>bbsdomain</paraKey>
				<paraValue>bbs.exmobi.xanway.com</paraValue>
				<paraName>ExMobi论坛地址</paraName>
				<paraMemo>ExMobi论坛地址</paraMemo>
				<paraRule></paraRule>
			</param>
		</paralist>
	</group>
</config>
```

config.xml文件中主要配置项说明

| Key值            类型   说明                                 |
| ------------------------------------------------------------ |
| groupId         属性   配置项组id                            |
| groupName  属性   配置项组名称                               |
| paraKey        节点  参数key值，该配置项的唯一key值，ParamConfig类需要根据该值获取到paraValue配置值 |
| paraMemo   节点  参数配置示例，用于介绍该配置项的配置示例，ExMobi管理端会读取并显示该值 |
| paraName    节点  参数名称，无特殊用途，ExMobi管理端会读取并显示该值 |
| paraRule       节点  参数值定义正则规则，暂不生效            |
| paraValue     节点  参数value值，ParamConfig类根据该paraKey值获取到的即是该值 |