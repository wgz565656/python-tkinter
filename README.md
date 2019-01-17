import tkinter as tk
import tkinter.messagebox
import pickle
import sys
import re
import pymysql
from  tkinter import ttk

# class Animal(object):
#         def __init__(self, name):
#                 self.name = name
window=tk.Tk()
window.title('欢迎来到爬书吧系统')
window.geometry('450x300+100+100')
# 画布放置图片
fname=sys.path[0]+r'C:\\Users\\11019\\ApythonTkinter\\frxxz.png'
canvas=tk.Canvas(window,height=300,width=500)
imagefile=tk.PhotoImage(file=r'C:\Users\11019\ApythonTkinter\frxxz.png')
image=canvas.create_image(0,0,anchor='nw',image=imagefile)
canvas.pack(side='top')

#标签用户名密码
tk.Label(window,text='用户名:').place(x=100,y=150)
tk.Label(window,text='密码:').place(x=100,y=190)

#用户名输入框
var_user_name=tk.StringVar()
entry_user_name=tk.Entry(window,textvariable=var_user_name)
entry_user_name.place(x=160,y=150)

#密码输入框
var_user_pwd=tk.StringVar()
entry_user_pwd=tk.Entry(window,textvariable=var_user_pwd,show='*')
entry_user_pwd.place(x=160,y=190)

#注册输入框
new_name=tk.StringVar()
new_pwd=tk.StringVar()
new_pwd_confirm=tk.StringVar()

new_No=tk.StringVar()
new_d=tk.StringVar()
new_p=tk.StringVar()
#bt_bookup

bookNo=tk.StringVar()
conn=pymysql.connect(host="127.0.0.1",port=3306,user="root",password="root",database="userbase",charset="utf8")
cursor=conn.cursor()
 #获取一个光标 并设置一个连接



def showBook():
        def book_up():
            Upbook=new_No.get()
            Upbookd=new_d.get() 
            Upbookp=new_p.get()
            conn=pymysql.connect(host="127.0.0.1",port=3306,user="root",password="root",database="userbase",charset="utf8")
            cursor=conn.cursor()
            sql_update="update booklist set bookDesc= '"+Upbookd+"' where bookNo="+Upbook
            print(sql_update)
            try:  
                cursor.execute(sql_update)  #像sql语句传递参数
                tk.messagebox.showinfo(title='更新成功',message='更新数据成功！恭喜你！')
                #提交  
                conn.commit()  
            except Exception as e:  
                #错误回滚  
                conn.rollback()   
            finally:  
                conn.close() 
        def show_up():
            bt_bookup=tk.Toplevel(window)
            bt_bookup.geometry('350x200')
            bt_bookup.title('更新图书')
            #用户名变量与标签、输入框

            tk.Label(bt_bookup,text='需要更新的书籍编号').place(x=10,y=10)
            tk.Entry(bt_bookup,textvariable=new_No).place(x=150,y=10)

            tk.Label(bt_bookup,text='请输入他的新的内容').place(x=10,y=50)
            tk.Entry(bt_bookup,textvariable=new_d).place(x=150,y=50)    
            #重复密码变量及标签、输入框

            tk.Label(bt_bookup,text='请输入他的新价格：').place(x=10,y=90)
            tk.Entry(bt_bookup,textvariable=new_p).place(x=150,y=90)    
            #确认注册按钮及位置
            bt_bookup=tk.Button(bt_bookup,text='确认修改',command=book_up)
            bt_bookup.place(x=150,y=130)
        def book_delate():
            bookNo1=bookNo.get()
            print(bookNo1)
            conn=pymysql.connect(host="127.0.0.1",port=3306,user="root",password="root",database="userbase",charset="utf8")
            cursor=conn.cursor()
            sql_delete="delete from booklist where bookNo = "+bookNo1
            
            print(sql_delete)
            try:
                cursor.execute(sql_delete)
                
                tk.messagebox.showinfo(title='删除成功',message='删除数据成功！恭喜你！')
            except Exception as e:
                conn.rollback()
            finally:
                conn.close()
        def show_delate():
            bt_bookdelete=tk.Toplevel(window)
            bt_bookdelete.geometry('350x200')
            bt_bookdelete.title('删除图书')
            #用户名变量与标签、输入框

            tk.Label(bt_bookdelete,text='请输入他的编号').place(x=10,y=50)
            tk.Entry(bt_bookdelete,textvariable=bookNo).place(x=150,y=50)    
            #重复密码变量及标签、输入框

            #确认注册按钮及位置
            bt_bookdelete=tk.Button(bt_bookdelete,text='确认删除',command=book_delate)
            bt_bookdelete.place(x=150,y=130)
            
        def book_sign_up():
            Nname=new_name.get()
            Npwd=new_pwd.get() 
            npf=new_pwd_confirm.get()
            conn=pymysql.connect(host="127.0.0.1",port=3306,user="root",password="root",database="userbase",charset="utf8")
            cursor=conn.cursor()
     #       all_data=selectAll()
    #         for i in range(len(all_data[0])):
    #             if all_data[i][1]==Nname:
    #                 tk.messagebox.showinfo(title='已有数据',message='已存在用户，请勿继续添加同名用户 ')
    #                 break;
    #             else:
    #                 tk.messagebox.showinfo(title='数据',message='正在插入数据')
    #                 break;
            cursor = conn.cursor()
            sql = "INSERT INTO booklist(bookName,bookDesc,bookPrice) VALUES (%s, %s, %s);"
            cursor.execute(sql,[Nname,Npwd,npf])
            conn.commit()
            tk.messagebox.showinfo(title='添加成功',message='添加数据成功！恭喜你！')
            print("添加书籍成功")  
        
        def showInsert():            #新建注册界面
            bt_bookinster=tk.Toplevel(window)
            bt_bookinster.geometry('350x200')
            bt_bookinster.title('新增图书')
            #用户名变量与标签、输入框

            tk.Label(bt_bookinster,text='图书名').place(x=10,y=10)
            tk.Entry(bt_bookinster,textvariable=new_name).place(x=150,y=10)

            tk.Label(bt_bookinster,text='请输入他的编号').place(x=10,y=50)
            tk.Entry(bt_bookinster,textvariable=new_pwd).place(x=150,y=50)    
            #重复密码变量及标签、输入框

            tk.Label(bt_bookinster,text='请输入他的类型：').place(x=10,y=90)
            tk.Entry(bt_bookinster,textvariable=new_pwd_confirm).place(x=150,y=90)    
            #确认注册按钮及位置
            bt_bookinster=tk.Button(bt_bookinster,text='确认添加',command=book_sign_up)
            bt_bookinster.place(x=150,y=130)
        
        def getBookAll_data():
            db=pymysql.connect(host="127.0.0.1",port=3306,user="root",password="root",database="userbase",charset="utf8")
            cur=db.cursor()
            sql='select * from booklist'
            try:
                cur.execute(sql)
                result_data=cur.fetchall()#获取所有的记录
                print(result_data)
#                print('bookNo','bookName','bookDesc','bookPrice')
                print(result_data[0][1])#3凡人修仙传
#                bookName=result_data[0][1]
                for i in range(len(result_data)):
                    tree.insert("",i+1,text=i,value=(result_data[i][0],result_data[i][1],result_data[i][2],result_data[i][3]))
                
#                 for row in 4:
#                     bookNo=result_data[row][0]
#                     bookName=result_data[row][1]
#                     bookDesc=result_data[row][2]
#                     bookPrice=result_data[row][3]
#                     print('bookNo','bookName','bookDesc','bookPrice')
            except Exception as e:
                raise e
            finally:
                    db.close()
        window_shouwBook=tk.Toplevel(window)
        window_shouwBook.geometry('650x400+100+100')
        window_shouwBook.title('图书馆页面')
        tree=ttk.Treeview(window_shouwBook)
       
        tree.pack()
        tree["columns"]=("bookNo","bookName","bookDesc","bookPrice",)#与heading的顺序相对应
        
        tree.column("bookNo",width=300)
        tree.column("bookName",width=300)
        tree.column("bookDesc",width=300)
        tree.column("bookPrice",width=300)

        tree.heading("bookNo",text="书编号")
        tree.heading("bookName",text="书名")
        tree.heading("bookDesc",text="书籍简介")
        tree.heading("bookPrice",text="书价格")
        getBookAll_data()
        bt_bookinster=tk.Button(window_shouwBook,text='新增',command=showInsert)
        bt_bookinster.place(x=0,y=30)
        
        bt_bookdelate=tk.Button(window_shouwBook,text='删除',command=show_delate)
        bt_bookdelate.place(x=0,y=100)
        
        bt_bookupdate=tk.Button(window_shouwBook,text='更改',command=show_up)
        bt_bookupdate.place(x=0,y=170)
        
        canvas=tk.Canvas(window_shouwBook,height=3000,width=5000)
        imagefile=tk.PhotoImage(file=r'C:\Users\11019\ApythonTkinter\frxxz22.png')
        image=canvas.create_image(20,20,anchor='nw',image=imagefile)
        canvas.pack(side='left')
        
        
def showView():            #新建注册界面
        window_sign_up=tk.Toplevel(window)
        window_sign_up.geometry('350x200')
        window_sign_up.title('注册')
        #用户名变量与标签、输入框
        
        tk.Label(window_sign_up,text='用户名：').place(x=10,y=10)
        tk.Entry(window_sign_up,textvariable=new_name).place(x=150,y=10)
        
        tk.Label(window_sign_up,text='请输入密码：').place(x=10,y=50)
        tk.Entry(window_sign_up,textvariable=new_pwd,show='*').place(x=150,y=50)    
        #重复密码变量及标签、输入框
        
        tk.Label(window_sign_up,text='请再次输入密码：').place(x=10,y=90)
        tk.Entry(window_sign_up,textvariable=new_pwd_confirm,show='*').place(x=150,y=90)    
        #确认注册按钮及位置
        bt_confirm_sign_up=tk.Button(window_sign_up,text='确认注册',command=user_sign_up)
        bt_confirm_sign_up.place(x=150,y=130)
def selectAll():
    sql = "SELECT USER_ID,name,pwd from userlist;"
    # 执行SQL语句
    cursor.execute(sql)
    # 获取多条查询数据
    userAllDate = cursor.fetchall()
    cursor.close()
    conn.close()
    # 打印下查询结果
    print(userAllDate[0][1])
    return userAllDate
#登录函数
def user_log_in():
    name=var_user_name.get()
    pwd=var_user_pwd.get()
#法一为作为文件存储，修改为数据库连接
#     try:
#         with open('usr_info.pickle','rb') as usr_file:
#             usrs_info=pickle.load(usr_file)
#     except FileNotFoundError:
#         with open('usr_info.pickle','wb') as usr_file:
#             usrs_info={'admin':'admin'}
#             pickle.dump(usrs_info,usr_file)
#去读取所有的内容
    conn=pymysql.connect(host="127.0.0.1",port=3306,user="root",password="root",database="userbase",charset="utf8")
    cursor=conn.cursor()
    
    sql='select * from userlist where name=%s and pwd=%s;'
#sql='select * from userlist where name=%s and pwd=%s and USER_NAME=%s;'
#定义一个要执行的sql语句，用%s作为占位符
    ret=cursor.execute(sql,[name,pwd])

#拼接字符串的sql语句，并去数据库执行
    
    if ret:
        tk.messagebox.showinfo(title='welcome',message='欢迎你    '+name)
        print("登录成功")
        showBook()
    else:
        tk.messagebox.showerror(message='密码错误')
        print("登录失败")
    if name=='' or pwd=='':
        tk.messagebox.showerror(message='账户或密码为空')
#     else:
#         is_signup=tk.messagebox.askyesno('欢迎','您现在还没有注册，是否先注册')
#         if is_signup:
#             user_sign_up()
    cursor.close()
    conn.close()
    #先都不关闭关闭；
    
#注册函数
def user_sign_up():
        Nname=new_name.get()
        Npwd=new_pwd.get() 
        npf=new_pwd_confirm.get()
        conn=pymysql.connect(host="127.0.0.1",port=3306,user="root",password="root",database="userbase",charset="utf8")
        cursor=conn.cursor()
        all_data=selectAll()
#         for i in range(len(all_data[0])):
#             if all_data[i][1]==Nname:
#                 tk.messagebox.showinfo(title='已有数据',message='已存在用户，请勿继续添加同名用户 ')
#                 break;
#             else:
#                 tk.messagebox.showinfo(title='数据',message='正在插入数据')
#                 break;
        cursor = conn.cursor()
        sql = "INSERT INTO userlist(name,pwd) VALUES (%s, %s);"
        cursor.execute(sql,[Nname,Npwd])
        conn.commit()
        tk.messagebox.showinfo(title='添加成功',message='恭喜你！添加图书成功')
        print("添加成功")   
            
                # 执行SQL语句
                 

            #再在此处改为 可进行批量提交
             #cursor.execute(sql,data)
              # 提交事务   
            #except Exception as e:
        #tk.messagebox.showerror(message='注册失败')
        #print("注册失败")
         #有异常回滚事务
          # conn.rollback()
def user_sign_quit():
        window.destroy()


bt_login=tk.Button(window,text='登录',command=user_log_in)
bt_login.place(x=140,y=230)
bt_logup=tk.Button(window,text='注册',command=showView)
bt_logup.place(x=210,y=230)
bt_logquit=tk.Button(window,text='退出',command=user_sign_quit)
bt_logquit.place(x=280,y=230)
#主循环
window.mainloop()
