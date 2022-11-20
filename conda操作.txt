# 创建虚拟环境：

conda create -n your_env_name python=X.X（2.7、3.6等）

(conda create --name your_env_name python=X.X 也可以)

conda create -n为关键字，your_env_name虚拟环境名称。指定python版本为2.7，注意至少需要指定python版本或者要安装的包， 在不指定python版本时，自动安装最新python版本。

# 激活虚拟环境：

1. 激活：activate your_env_name(虚拟环境名称)

2. 退出：conda deactivate 虚拟环境：

3. 切回到root环境：activate root

4. 删除虚拟环境：conda remove -n your_env_name(虚拟环境名称) --all

5. 删除envs中的包：conda remove --name yourenvnamepackage_name（包名）
6. pip国内的一些镜像:

   1. 阿里云 http://mirrors.aliyun.com/pypi/simple/ 

   2. 中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/ 

   3. 豆瓣(douban) http://pypi.douban.com/simple/ 

   4. 清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/ 

   5. 中国科学技术大学 http://pypi.mirrors.ustc.edu.cn/simple/

7. 临时使用某个源安装版本:

8. pip install -i https://pypi.douban.com/simple torch==1.5.0

9. anaconda 添加国内安装源:

   1. conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/

   2. conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/

   3. conda config --set show_channel_urls yes

10. 显示目前设置的所有镜像源，在执行conda config 命令的时候，会在当前用户目录下创建 .condarc 文件，可以查看更换源前后该文件内容的变化:

11. 查看当前环境的源配置：conda config --show-sources

12. 删除安装源：conda config --remove channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/

13. 换回默认源：conda config --remove-key channels




# notes：

1. 创建虚拟环境前，不用指定python环境；创建虚拟环境后，conda 安装一个包后，会自动将其他环境的包给复制过来

2. 若是sklearn出现import错误，则,以解决不兼容的问题

```python
pip uninstall scikit-learn  
pip uninstall scipy
pip  install --index https://pypi.mirrors.ustc.edu.cn/simple/ scipy  
pip  install --index https://pypi.mirrors.ustc.edu.cn/simple/ scikit-learn
```

3. 如果在虚拟环境import失败，就cmd在控制台激活虚拟环境，并重新pip,还有一个原因就是setting.json设置的python路径不是虚拟环境路径，因此确实相应的包。

