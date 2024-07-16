# planet_atm
## 系外行星大气
### 课题目标
#### • 从凌星的高分辨率光谱观测数据中提取行星大气强线（如Na双线、H-alpha线）的透射谱
#### • 掌握对恒星光谱中的地球大气吸收线进行修正的方法
#### • 理解凌星期间恒星谱线轮廓发生形变的来源及其正向建模方法
### 参考文献
#### • 本课题可参考Sicilia et al. (2022)，该文对数据处理分析流程进行了详细介绍，并提供了开源的软件包SLOPpy ◦ https://ui.adsabs.harvard.edu/abs/2022A%26A...667A..19S/abstract
### 数据获取
#### • 由3.6米TNG望远镜的HARPS-N光谱仪观测的KELT-20b的三次凌星光谱数据，分别观测于2017-8-16、2018-7-12、2018-7-19这三晚，可从TNG archive网站下载
##### ◦ http://archives.ia2.inaf.it/tng/
#### • 以2017-8-16为例，在该网页“Name resolver”处输入KELT-20后点击Resolve，在“Observation date”处输入2017-08-16和2017-08-18，在“Instrum”处选择HARPN，点击Search，待结果列表出现后，点击Download选取URL list进行下载。
##### ◦ 该list里需要下载的文件有三种，分别是文件名含e2ds、s1d、ccf的文件，可以使用wget进行下载
##### ◦ 下载完成后，打开任一e2ds的文件头文件，查看该夜观测对应的blaze文件是什么，然后补足网址后用wget下载，同样，将blaze替换为lamp后也用wget下载
###### fold any-e2ds-file | grep BLAZE
###### wget http://archives.ia2.inaf.it/files/tng/your-blaze-file.gz
## 软件准备
### SLOPpy：数据处理分析与透射谱生成
#### • github网站：https://github.com/LucaMalavolta/SLOPpy/tree/main
#### • Documentation网站：https://sloppy.readthedocs.io/en/latest/
#### • 截止ABDEC2024，建议按Documentation网站上给出的流程，新建conda环境，然后以git clone的方式进行安装
##### ◦ 需要使用pip install -r extra_requirements.txt将额外的依赖安装完成
##### ◦ 若未进行依赖安装，在软件运行过程中，可能在调用scikit-learn、PyDE时报错，在报错时补充安装可继续进行后续步骤
##### ◦ 若SLOPpy以非git clone方式安装（这个对应于版本1.3，pip安装的是1.2.2），可能产生numpy.int报错，解决方案可以是重新以git clone方式安装SLOPpy，或者手动将产生报错的numpy.int修改掉
### Molecfit：地球大气改正
#### • SLOPpy提供了多种地球大气改正的方法，其中一种是采用molecifit对光谱数据进行拟合并加以改正，软件运行时预先生成molecfit所需的配置文件，然后调用molecfit运行，因此需要提前安装molecfit
##### ◦ 注意，安装完molecfit，也可单独使用molecfit，而不是必须使用SLOPpy调用
#### • 安装网站可参见
##### ◦ https://www.eso.org/sci/software/pipelines/molecfit/molecfit-pipe-recipes.html
#### • 新版Molecfit已集成到ESO软件中，截止ABDEC2024的最新版本为4.3.3
##### ◦ Mac OS安装时推荐使用MacPorts进行安装，在上方的ESO网站有安装说明的介绍页面
#### • 此外也有独立于ESO软件的老版本，最高版本号为1.5.9，其安装可能有一定的操作系统版本依赖性问题
##### ◦ Mac OS下的安装可以参考IAC的这个网页：
###### ▪ https://research.iac.es/sieinvens/siepedia/pmwiki.php?n=Tutorials.MolecfitDocker
#### • 在SLOPpy中，老版的独立版本molecfit的相关模块带有v1标签，但如果安装的是新版的集成版本molecfit，则需要在SLOPpy的yaml配置文件中做如下修改：
##### ◦ 去掉_v1
##### ◦ 在molecfit一节，将installation_path: /Applications/molecfit/bin/替换为esorex_exec: esorex
##### ◦ 在esorex_exec下面新添一行：aer_version: 3.8.1.2
### PySME：生成恒星模型光谱和强度谱，用于模拟CLV+RM效应
#### • SLOPpy在正向模拟CLV+RM效应时，需要提前提供盘积分的恒星模型流量谱和有出射方向依赖的强度谱文件，可由PySME生成
#### • 安装网站可参见 ◦ https://github.com/MingjieJian/SME
#### • 建议单独建立conda环境采用git clone模式进行安装，目前的python版本要求为3.7-3.11
#### • Macbook Pro非intel芯片安装后，使用时可能会遇到架构问题而无法使用，可以尝试使用Rosetta来解决
#### • 使用PySME时需要提前准备线表，可使用VALD3线表，需提前注册账号：◦ http://vald.astro.uu.se/~vald/php/vald.php















