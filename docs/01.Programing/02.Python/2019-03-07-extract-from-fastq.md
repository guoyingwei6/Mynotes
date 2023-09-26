---
layout: post
title: "从fastq文件中批量提取/过滤序列"
author: "Yingwei Guo"
categories: journal
tags: [documentation,sample]
image: 
---

> 博主也是刚刚接触生信，会将自己平时练习用到的python脚本发布到博客上，用来记录自己的学习之路。
# 介绍
测序回来的fastq文件通常在分析之前，需要进行过滤，该脚本利用python实现fastq文件中提取指定ID的序列，并保存为新fastq文件，脚本一所有输入文件和输出文件都必须是压缩文件格式，脚本二是否压缩均可，但输出的过滤后的文件会和原始fastq文件保持一致。
# 说明
**脚本一：**
- 输入文件为fq.gz文件，压缩的ID list文件。
- 必须是压缩格式的文件才可以，如果非压缩格式，可以压缩成gz格式后使用该脚本，或者将该脚本改写。

**脚本二：**
- 自动识别input文件格式，无论fq文件是否压缩都可以打开。
- 输出文件格式默认与fq文件一致，即fq是压缩文件，过滤后的输出文件也是压缩文件，fq没有压缩，输出文件也是普通格式。
# 脚本如下
**脚本一**




```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
#Author: gyw
#Date: 2019-03-07
#E-mail: willgyw@126.com
#Description:Filter reads from fastq.gz 

import gzip
import argparse


parser = argparse.ArgumentParser(description='filter reads from fastq.gz')
parser.add_argument('--fastq', '-q', dest='fastq', help='input a fastq.gz file')
parser.add_argument('--idlist', '-i', dest='idlist', help='input idlist.gz file')
parser.add_argument('--outfile', '-o', dest='outfile', help='input outfile name,end by gz')
args = parser.parse_args()

fastqdict = {}

with gzip.open(args.fastq, 'rb') as fastq:
    for line in fastq:
        if line.decode().startswith('@'):
            fastqid = line.decode().strip().split()[0][1:]
            fastqdict[fastqid] = ''
        else:
            fastqdict[fastqid] += line.decode()
   
outfile = gzip.open(args.outfile, 'wb')
       
with gzip.open(args.idlist, 'rb') as idfile:
    for line in idfile:
        readsid = line.decode().strip()
        for key in fastqdict.keys():
            if readsid == key:
                res = '@' + key + '\n' + fastqdict[key]
                outfile.write(res.encode())

outfile.close()


```
**脚本二**
```python
import filetype
import gzip
import argparse


parser = argparse.ArgumentParser(description='filter reads from fastq')
parser.add_argument('--fastq', '-q', dest='fastq', help='input a fastq file')
parser.add_argument('--idlist', '-i', dest='idlist', help='input idlist file')
parser.add_argument('--outfile', '-o', dest='outfile', help='input outfile name')
args = parser.parse_args()

fastqdict = {}
kind1 = filetype.guess(args.fastq)
kind2 = filetype.guess(args.idlist)
if kind1 is None:
    with open(args.fastq,'r') as fastq:
        for line in fastq:
            if line.startswith('@'):
                fastqid = line.strip().split()[0][1:]
                fastqdict[fastqid] = ''
            else:
                fastqdict[fastqid] += line
elif kind1.extension == 'gz':
    with gzip.open(args.fastq, 'rb') as fastq:
        for line in fastq:
            if line.decode().startswith('@'):
                fastqid = line.decode().strip().split()[0][1:]
                fastqdict[fastqid] = ''
            else:
                fastqdict[fastqid] += line.decode()
if kind1 is None:
    outfile = open(args.outfile, 'w')
elif kind1.extension == 'gz':
    outfile = gzip.open(args.outfile, 'wb')
if kind2 is None:
    with open(args.idlist, 'r') as idfile:
        for line in idfile:
            readsid = line.strip()
            for key in fastqdict.keys():
                if readsid == key:
                    res = '@' + key + '\n' + fastqdict[key]
                    if kind1 is None:
                        outfile.write(res)
                    elif kind1.extension == 'gz':
                        outfile.write(res.encode())
elif kind2.extension == 'gz':
    with gzip.open(args.idlist, 'rb') as idfile:
        for line in idfile:
            readsid = line.decode().strip()
            for key in fastqdict.keys():
                if readsid == key:
                    res = '@' + key + '\n' + fastqdict[key]
                    if kind1 is None:
                        outfile.write(res)
                    elif kind1.extension == 'gz':
                        outfile.write(res.encode())

outfile.close()

```

如有错误，欢迎大家指正。