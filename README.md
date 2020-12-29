电话号码查询系统
==============
# 一、运行环境
dev-c++
# 二、详细设计
## 2.1 头文件，双链表，结构体的建立
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <conio.h>
struct record { //建立结构体
	char name[20];
	char tel[20];
	char email[20];
	char qq[20];
	char rela[20];
} student[1000];
struct slnode { //建立双向链表
	record date;
	struct slnode *next;
	struct slnode *prior;
};
typedef slnode * linklist;
linklist l;
static int num=0;
FILE *fp;
int n1=0;
```
## 2.2主函数算法
### 2.2.1 main()函数的算法
```
int main() {
	initlist();
	load();
	listinsert();
	while (1)
		mainmenu();
	return 0 ;
}
```
### 2.2.2 mainmenu()函数算法
```
void mainmenu() { //主菜单
	char choic;
	system("cls"); //清屏
	printf("\n\t\t***************欢迎进入电话号码查询系统***************");
	printf("\n\t\t*********************1-新添联系人 ********************");
	printf("\n\t\t*********************2-查找联系人 ********************");
	printf("\n\t\t*********************3-删除联系人 ********************");
	printf("\n\t\t*********************4-保存并退出 ********************");
	printf("\n\t\t*********************5-不保存退出 ********************");
	printf("\n\t\t******************************************************");
	printf("\n\t\t 请选择：");
	choic=getch();
	switch (choic) {
		case '1':
			enter();
			break;
		case '2':
			searchmenu();
			break;
		case '3':
			delet();
			break;
		case '4':
			save();
			break;
		case '5':
			exit(0);
		default:
			mainmenu();
	} //switch 语句选择
}
```
### 2.2.3 searchmenu()函数算法
```
void searchmenu() { //查询菜单
	char choic;
	system("cls"); //清屏
	printf("\n\t\t******************* 查询菜单 *******************");
	printf("\n\t\t**************** 1-显示所有信息 ****************");
	printf("\n\t\t**************** 2-按姓名查询 ******************");
	printf("\n\t\t**************** 3-返回主菜单 ******************");
	printf("\n\t\t************************************************");
	printf("\n\t\t 请选择：");
	choic=getch();
	switch (choic) {
		case '1':
			display();
			printf("ok");;
			break;
		case '2':
			search();
			break;
		case '3':
			mainmenu();
			break;
	}
}
```
### 2.2.4 创建新的通讯录 enter()函数算法
```
void enter() { //添加纪录
	printf("\n\t\t**************** 请输入信息 ****************\n");
	printf("\n\t\t姓名:");
	scanf("%s",&student[num].name);
	printf("\n\t\t电话:");
	scanf("%s",&student[num].tel);
	printf("\n\t\temail:");
	scanf("%s",&student[num].email);
	printf("\n\t\tqq:");
	scanf("%s",&student[num].qq);
	printf("\n\t\t分组:");
	scanf("%s",&student[num].rela); //将联系人信息添加到数组 student
	num++;
	printf("\n\t\t是否继续添加?(Y/N):"); //判断是否继续添加
	if (getch()=='y')
		enter();
	return; //返回函数
}
```
### 2.2.5 显示通讯录信息 display()函数算法
```
void display() { //显示所有
	int i;
	system("cls");//清屏
	if(num!=0) {
		printf("\n\t\t*************** 以下为所有信息 ***************");
		for (i=0; i<num; i++) {
			printf("\n\t\t 姓名： %s",student[i].name);
			printf("\n\t\t 电话： %s",student[i].tel);
			printf("\n\t\temail： %s",student[i].email);
			printf("\n\t\tqq： %s",student[i].qq);
			printf("\n\t\t 分组： %s",student[i].rela);
			printf("\t\t");
			if (i+1<num) {
				printf("\n\t\t__________________________");
				system("pause"); //调用 pause 函数显示按任意键继续
			}
		}
		printf("\n\t\t************************************************");
	} else
		printf("\n\t\t 通讯录中无任何纪录");
	printf("\n\t\t 按任意键返回主菜单：");
	getch();
	return;
}
```
### 2.2.6 保存通讯录 save()函数算法
```
void save() { //写入文件
	int i;
	if ((fp=fopen("student.dat","wb"))==NULL) { //以写二进制方式打开文件 student，并使 fp 指向该文件。
		printf("\n\t\t 文件打开失败");
		exit(1);
	}
	for (i=0; i<num; i++) {
		fwrite(&student[i],sizeof(struct record),1,fp); //在 fp所指向文件写入一个数据存储到数组 student中直到文件结束
	}
	fclose(fp); //关闭文件
	printf("\n\t\t 通讯录文件已保存");
	printf("\n\t\t 按任意键退出程序\n\t\t");
	system("pause");
	exit(0);
}
```
### 2.2.7 增加一个节点 listinsert()函数算法
```
void listinsert() { //增加一个结点
	linklist s,p=l;
	for(int i=0; i<num; i++) {
		s=new slnode;
		strcpy(s->date.name,student[i].name);
		strcpy(s->date.tel,student[i].tel);
		strcpy(s->date.email,student[i].email);
		strcpy(s->date.qq,student[i].qq);
		strcpy(s->date.rela,student[i].rela); //复制数组
		s->prior=p->prior;
		s->next=p;
		p->prior->next=s;
		p->prior=s;
		p=p->next;
	}
}
```
### 2.2.8 建立头结点
```
void initlist() { //建立头结点
	l=new slnode;
	l->next=l;
	l->prior=l;
}
```
### 2.2.9 查找 search()函数算法
```
void search() {
	int j=0,a=0;
	linklist p=l;
	printf("\n\t\t***************** 按姓名查找 *******************");
	char name[20];
	printf("\n\t\t 请输入姓名:");
	scanf("%s",name);
	printf("\t");
	printf("\n\t\t 查询到的信息:");
	for(int i=a; i<num; i++) {
		if(strcmp(name,student[i].name)==0) { //两字符串相等
			printf("\n\t\t________________________________");
			printf("\n\t\t");
			printf("姓名:");
			printf("%s",student[i].name);
			printf("\n\t\t");
			printf("电话:");
			printf("%s",student[i].tel);
			printf("\n\t\t");
			printf("email:");
			printf("%s",student[i].email);
			printf("\n\t\t");
			printf("qq:");
			printf("%s",student[i].qq);
			printf("\n\t\t");
			printf("分组:");
			printf("%s",student[i].rela);
			j++;
		} else
			continue;
	}
	printf("\n\t\t 查找到的人数%d",j);
	if(j==0) {
		printf("\n\t\t 通讯录中没有此人!");
	}
	getch();
}
```
### 2.2.10 删除 delete()函数算法
```
void delet() {
	int a=0;
	int findmark=0;
	int j;
	int deletemark=0;
	int i;
	char name[20];
	printf("\n\t\t 请输入要删除的联系人姓名：");
	scanf("%s",name);
	for (i=a; i<num; i++) {
		if (strcmp(student[i].name,name)==0) {
			printf("\n\t\t 以下是您要删除的联系人信息：");
			findmark++;
			printf("\n\t\t________________________________");
			printf("\n\t\t 姓名： %s",student[i].name);
			printf("\n\t\t 电话： %s",student[i].tel);
			printf("\n\t\temail： %s",student[i].email);
			printf("\n\t\tqq： %s",student[i].qq);
			printf("\n\t\t 分组: %s",student[i].rela);
			printf("\n\t\t________________________________");
			printf("\n\t\t 是否删除?(y/n)");
			if (getch()=='y') {
				for (j=i; j<num-1; j++)
					student[j]=student[j+1];
				num--;
				deletemark++;
				printf("\n\t\t 删除成功");
				if((i+1)<num) {
					printf("\n\t\t 是否继续删除相同姓名的联系人信息?(y/n)");
					if (getch()=='y') {
						a=i;
						continue;
					}
				}
				printf("\n\t\t 是否继续删除?(y/n)");
				if (getch()=='y')
					delet();
				return;
			}
			if((i+1)<num) {
				printf("\n\t\t 是否继续删除相同姓名的联系人信息?(y/n)");
				if (getch()=='y') {
					a=i;
					continue;
				}
			}
		} else
			continue;
	}
	if ((deletemark==0)&&(findmark==0)) {
		printf("\n\t\t 没有该联系人的纪录");
		getch();
		return;
	} else if (findmark!=0) {
		printf("\n\t\t 没有重名信息");
		printf("\n\t\t 没有该联系人的纪录");
		getch();
		return;
	}
}
```
### 2.2.11 打开文件 load()函数算法
```
void load() {
	if((fp=fopen("student.dat","rb"))==NULL) {
		printf("\n\t\t 通讯录文件不存在"); //通讯录不存在
		if ((fp=fopen("student.dat","wb"))==NULL) {
			printf("\n\t\t 建立失败"); //建立失败
			exit(0);
		} else {
			printf("\n\t\t 通讯录文件已建立");
			printf("\n\t\t 按任意键进入主菜单");
			getch();
			return;
		}
		exit(0);
	}
	fseek(fp,0,2); //将位置指针从文件尾移动 0 个字符
	if (ftell(fp)>0) { //流式文件中的指针的当前位置大于零
		rewind(fp);
		for (num=0; !feof(fp) && fread(&student[num],sizeof(struct record),1,fp); num++); //在 fp 所指向文件读入一个数据存储到数组 student 中直到文件结束
		printf("\n\t\t 文件导入成功");
		printf("\n\t\t 按任意键返回主菜单");
		getch();
		return;
	}
	printf("\n\t\t 文件导入成功");
	printf("\n\t\t 通讯录文件中无任何纪录");
	printf("\n\t\t 按任意键返回主菜单");
	getch();
	return;
}
```
# 三、使用说明
## 3.1 显示菜单
![](https://s3.ax1x.com/2020/12/29/rbyBp8.png)
## 3.2 新建联系人
在主菜单下，用户输入1并回车，然后按照提示输入数据，分别输入姓名、电话、email和分组，运行结果如图所示：
![](https://s3.ax1x.com/2020/12/29/rbREZQ.png)
## 3.3 查找联系人
### 3.3.1 查找菜单的显示
在主菜单下，用户输入2并回车。可以看到查询菜单如下图所示;
![](https://s3.ax1x.com/2020/12/29/rbg1pV.png)
### 3.3.2 显示所有信息
在查询菜单下，用户输入1，可以显示所有联系人信息
![](https://s3.ax1x.com/2020/12/29/rbgIc8.png)
### 3.3.3 按姓名查询
在查询菜单下，用户输入2，可以通过姓名查询联系人信息
![](https://s3.ax1x.com/2020/12/29/rb2A41.png)
## 3.4 删除联系人
在主菜单下，用户输入3并回车，进行记录的删除。本系统按姓名的方式删除，运行结果如下图所示：
![](https://s3.ax1x.com/2020/12/29/rb2aDg.png)
## 3.5 保存并退出
在主菜单下，用户输入4并回车，可将信息保存在本地，生成student.dat文件
