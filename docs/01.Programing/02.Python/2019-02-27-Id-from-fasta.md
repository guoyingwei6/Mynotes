---
layout: post
title: "根据ID从FASTA文件中提取序列"
author: "Yingwei Guo"
categories: journal
tags: [documentation,sample]
image: 
---

> 博主是一个刚刚接触生信的新手，正在学习Linux和Python，偶尔会在该博客上面发布自己练习编程写的脚本，用来记录自己的学习之路。

## 介绍
根据序列的ID号从FASTA文件中批量提取序列是在平时常常要做的工作，Linux当中grep和awk工具、Perl语言和Python语言，以及samtools等都可以实现，以下是博主用Python实现的从FASTA文件中批量提取序列的脚本。

## 说明
- 需要用到fasta文件和ID的list文件。
- 所要提取的序列的ID需要提前写进一个文件中，每行一个。

## 脚本如下
**1. 采用*click*模块添加命令行参数。**

```python3
# -*- coding: utf-8 -*-
"""
@author: gyw
@Date:  Wed Feb 27 09:19:54 2019
@E-mail: willgyw@126.com
@Description: This program can extract sequences 
              from a fasta file, according to a 
              given id list.
"""

import click

@click.command()
@click.option('-f', '--fastafile', help='Input a fasta file', required=True)
@click.option('-i', '--idfile', help='Input an idlist', required=True)
@click.option('-o', '--outfile', help='Input the name of result file', default='result.fa')


def main(fastafile, idfile, outfile):
    """ 
    Extract seqences from a fasta file 
    according to a id list.
    """
    idfile = open(idfile, 'r')
    resultfile = open(outfile, 'w')
    for id in idfile:
        qid = id.strip()
        flag = 0 
        with open(fastafile,'r') as ffile:
            for line in ffile:
                line = line.strip()
                if line.startswith('>'):
                    name = line.replace('>','').split()[0]
                    if name == qid:
                        flag = 1
                        resultfile.write(line + '\n')
                    else:
                        flag = 0
                else:
                    if flag == 0:
                        pass
                    else:
                        resultfile.write(line + '\n')
    resultfile.close()


if __name__ == '__main__':
    main()


```
**2. 另一种方法，将FASTA文件中的序列导入*字典*中进行查找，速度远快于上一种。**
```python
# -*- coding: utf-8 -*-
"""
@author: gyw
@Date:  Wed Feb 28 2019
@E-mail: willgyw@126.com
@Description: A script to extract sequences from fasta file.
"""
import sys 

def usage():
    print('Usage: python3 script.py [fasta_file] [idlist_file] [outfile_name]')


def main():
    outf = open(sys.argv[3],'w')
    dict = {}
    with open(sys.argv[1], 'r') as fastaf:
        for line in fastaf:
            if line.startswith('>'):
                name = line.strip().split()[0][1:]
                dict[name] = ''
            else:
                dict[name] += line.replace('\n','')
     
    with open(sys.argv[2],'r') as listf:
        for row in listf:
            row = row.strip()
            for key in dict.keys():
                if key == row:
                    outf.write('>' + key+ '\n')
                    outf.write(dict[key] + '\n')
    outf.close()


try:
    main()
except IndexError:
    usage()

```
**3. 第三种方法，与第二种方法实现功能的核心算法一样，加入了*getopt*模块，可添加命令行参数。**

```python

import sys
import getopt


def usage():
    print('''
          Usage: python3 script.py [option] [patameter]
          -f/--fasta          input a fasta file
          -i/--idlist         input id list
          -o/--outfile        input outfile name
          -h/--help           show possible options
          ''')
    
opts,args = getopt.getopt(sys.argv[1:], 'hf:i:o:', ['help', 'fasta=', 'idlist=', 'outfile='])
for opt,val in opts:
    if opt == '-f' or opt == '--fasta':
        fasta = val 
    elif opt == '-i' or opt == '--idlist':
        idlist = val 
    elif opt == '-o' or opt == '--outfile':
        outfile = val 
    elif opt == '-h' or opt == '--help':
        usage()
        sys.exit(1)

outf = open(outfile, 'w')

dict = {}
with open(fasta, 'r') as fastaf:
    for line in fastaf:
        if line.startswith('>'):
            name = line.strip().split()[0][1:]
            dict[name] = ''
        else:
            dict[name] += line.replace('\n','')

with open(idlist, 'r') as listf:
    for row in listf:
        row = row.strip()
        for key in dict.keys():
            if key == row:
                outf.write('>' + key + '\n')
                outf.write(dict[key] + '\n')

outf.close()

```
**4. 实现功能的算法与方法一相同，通过*fire*模块解析命令行参数。**

```python
import fire


def main(fastafile, idfile, outfile):
    """
    Extract seqences from a fasta file 
    according to a id list.
    """
    idfile = open(idfile, 'r')
    resultfile = open(outfile, 'w')
    for id in idfile:
        qid = id.strip()
        flag = 0
        with open(fastafile, 'r') as ffile:
            for line in ffile:
                line = line.strip()
                if line.startswith('>'):
                    name = line.replace('>','').split()[0]
                    if name == qid:
                        flag = 1
                        resultfile.write(line + '\n')
                    else:
                        flag = 0
                else:
                    if flag == 0:
                        pass
                    else:
                        resultfile.write(line + '\n')
    resultfile.close()


fire.Fire(main)

```

博主也是刚刚学习Python，如有错误，欢迎指正~