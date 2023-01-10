---
title: Building A Windows Server infrastructure For Data Analysis
date: 2023-01-11 02:38:19
categories:
- Python
- Basic
tags:
- Python
- Machine Learning
- Deep Learning
- PyTorch
- TensorFlow
mathjax: true
---
# SSH 설정 및 포트포워딩

[SSH Setup](https://www.lainyzine.com/ko/article/how-to-run-openssh-server-and-connect-with-ssh-on-windows-10/)

윈도우 컴퓨터에서 위 설정을 모두 마쳤다면 컴퓨터와 연결된 공유기의 gateway에 접속하여 포트포워딩 진행

![Port forwarding](https://user-images.githubusercontent.com/42334717/211630306-b2da0277-1984-40dc-9093-7f122facd71c.png)

+ ssh 사용을 위한 `22` 포트와 jupyter notebook 사용을 위한 `88` 포트를 포워딩

~~~sh
ssh username@XXX.XXX.XXX.XXX -p {port}
~~~

<!-- More -->

***

# Anaconda 설치

[Anaconda](https://www.anaconda.com/)

위의 홈페이지에서 anaconda 설치 후 아래 환경 변수 추가 (ssh에서 `conda` 명령어를 쓰기 위함)

> 환경 변수 설정 (Path)

~~~
C:\Users\username\anaconda3
C:\Users\username\anaconda3\Library
C:\Users\username\anaconda3\Scripts
C:\Users\username\anaconda3\Library\bin
~~~

> 가상 환경 생성

~~~python
conda create -n analysis python=3.7
conda activate analysis
~~~

---

# Library 설치

~~~python
conda install -y jupyter
conda install -y scikit-learn
~~~