# China_pyramid

以下是一个之前在网络上引起巨大轰动的人口金字塔动态图：<br>

![chart2](https://github.com/ljtyduyu/China_pyramid/blob/master/Image/%E4%BA%BA%E5%8F%A3%E9%87%91%E5%AD%97%E5%A1%94%EF%BC%881950-2050%EF%BC%89.gif)


没有找到原始数据，后来在UN的官网上找到了一个五年间隔的数据，顺便还原了下图表：<br>


```r
library(ggplot2)
library(animation)
library(dplyr)
library(tidyr)
library(xlsx)
library(ggthemes)
setwd("E:/数据可视化/R/R语言学习笔记/可视化/ggplot2/商务图表/动态图表")
```

```r
female<-read.xlsx("Population.xlsx",sheetName="Female",header=T,encoding='UTF-8',check.names = FALSE)
male<-read.xlsx("Population.xlsx",sheetName="Male",header=T,encoding='UTF-8',check.names = FALSE)

female<-female%>%gather(Year,Poputation,-1)
male<-male%>%gather(Year,Poputation,-1)
female$Poputation<-female$Poputation*-1

male$sex<-"male";female$sex<-"female"
```


```r
China_Population<-rbind(male,female)%>%mutate(abs_pop=abs(Poputation))
China_Population$agegroup<-factor(China_Population$agegroup,
levels=c("0-4","5-9","10-14","15-19","20-24","25-29","30-34","35-39","40-44","45-49","50-54","55-59","60-64","65-69","70-74","75-79","80+") ,order=T)

m<-seq(1950,2015,by=5)
```


```r
saveGIF({
  for (i in m) { 
    title <- as.character(i)
    year_data <- filter(China_Population,Year==i)
     g1<-ggplot(year_data,aes(x =agegroup,y=Poputation,fill=sex,width=1)) +
        coord_fixed()+ 
        coord_flip() +
        geom_bar(data=subset(year_data,sex=="female"),stat = "identity") +
        geom_bar(data=subset(year_data,sex=="male"), stat = "identity") +
        scale_y_continuous(breaks = seq(-70000,70000,length=9),
                         labels = paste0(as.character(c(abs(seq(-70,70,length=9)))), "m"), 
                         limits = c(-75000, 75000)) +
        theme_economist(base_size = 14) + 
        scale_fill_manual(values = c('#D40225', '#374F8F')) + 
        labs(title=paste0("Population structure of China:", title),
        caption="Data Source:United Nations Department of Economic and Docial Affairs\nPopulation Division\nWorld Population Prospects,the 2015 Revision"
        ,y="Population",x="Age") + 
        guides(fill=guide_legend(reverse = TRUE))+
        theme(
             legend.position =c(0.8,0.9),
             legend.title = element_blank(),
             plot.title = element_text(size=20),
             plot.caption = element_text(size=12,hjust=0),
         )

        print(g1)
  }
},movie.name='japan_pyramid.gif',interval=0.5,ani.width=700,ani.height=600)
```

![chart1](https://github.com/ljtyduyu/China_pyramid/blob/master/Image/China_pyramid.gif)


联系方式：
----------------------------------------------------
wechat：ljty1991  <br>
Mail:578708965@qq.com <br>
个人公众号：数据小魔方（datamofang） <br>
团队公众号：EasyCharts <br>
qq交流群：[魔方学院]553270834

个人简介：
-------------------------------------------------
**杜雨** <br>
财经专业研究僧； <br>
伪数据可视化达人； <br>
文科背景的编程小白； <br>
喜欢研究商务图表与地理信息数据可视化，爱倒腾PowerBI、SAP DashBoard、Tableau、R ggplot2、Think-cell chart等诸如此类的数据可视化软件，创建并运营微信公众号“数据小魔方”。 <br>
Mail:578708965@qq.com <br>

<div  align="center">    
<img src="https://github.com/ljtyduyu/FontMap-of-China/blob/master/Image/resume.png" width = "550" height = "300" alt="resume" align=center />
</div>

-------------------------------------------

备注信息：
----------------------------------------------------
<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" /></a><br />本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/">知识共享署名-非商业性使用 4.0 国际许可协议</a>进行许可。

