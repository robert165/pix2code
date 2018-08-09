FROM ubuntu:16.04

ADD sources.list /etc/apt/sources.list

# Install basic dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
        build-essential 

RUN apt-get install -y python-qt4

#安装anaconda2
# Install anaconda for python 2.7
#RUN wget --quiet https://repo.continuum.io/archive/Anaconda2-5.2.0-Linux-x86_64.sh -O ~/anaconda.sh 
ADD Anaconda2-5.1.0-Linux-x86_64.sh /tmp/anaconda.sh
RUN bash /tmp/anaconda.sh  -b  -p /opt/conda 
RUN echo "export PATH=/opt/conda/bin:$PATH" >> ~/.bashrc
ENV PATH /opt/conda/bin:$PATH
RUN rm /tmp/anaconda.sh && \
    conda clean -a -y 

## 使用清华的源
RUN conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/ && \
    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/ && \
    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r/ && \
    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/mro/ && \
    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/ && \
    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/ && \
    conda config --set show_channel_urls yes


#安装软件
RUN conda create -n pix2code python=2.7 pip scipy numpy PIL jupyter
RUN ["/bin/bash", "-c", "source activate pix2code"]
# Install requirements before copying project files

COPY requirements.txt .
#RUN /opt/conda/bin/conda install -q -y conda numpy scipy pip pillow
RUN /opt/conda/bin/python -m pip install -q -r "requirements.txt"


#解决docker终端不能输出中文问题
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
# python命令输出默认为ASCII，需要设定变量PYTHONIOENCODING为UTF-8
ENV PYTHONIOENCODING UTF-8

WORKDIR /workspace
EXPOSE 5000

