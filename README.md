# stock-management-system
#a simple tool to manage stock
#code


import datetime
tdate=datetime.date.today()
print(tdate)
import pickle

print("welcome")


#for purchase entry
def purchase():
    idno=int(input("enter idno:"))
    name=input("enter name:")
    a=check(idno,name)
    if a:
        p=open("purchase.dat","ab+")
        qty=int(input("quantity:"))
        price=int(input("price"))
        tax=int(input("tax rate:"))
        l=[idno,name,qty,price,tax]
        pickle.dump(l,p)
        print("value stored")
        p.close()
    else:
        pass
    ask("p")


#to check for repeat entry
def check(a,b):
    ct=0
    p=open("purchase.dat","rb")
    try:
        while True:
            obj=pickle.load(p)
            if obj[0]==a:
                print("repeat of value enter another")
                print(obj)
                ct=1
                
               
            elif obj[1]==b:
                print("repeat of value enter another")
                ct=1
                
            else:
                pass
    except EOFError:
        if ct==0:
            return True
        else:
            return False
            
        p.close()
        pass
    


def delete():
    ct=0
    print("enter details of item to be deleted")
    de=int(input("enter idno.:"))
    a=input("enter name:")
    p=open("purchase.dat","rb")
    u=open("pur2.dat","ab")
    try:
        while True:
            obj=pickle.load(p)
            if obj[0]==de and obj[1]==a:
                print("item found")
                ct=1
    
            else:
                pickle.dump(obj,u)
                print(obj)
                
    except EOFError:
        u.close()    
        p.close()
        if ct==0:
            print("item not found")
        else:
            copy()
            print("item deleted")
       
        u=open("pur2.dat","wb")
        u.close()
   
    ask("d")

    
#for sale of items    
def sale():
    ct=0
    buyer=input("enter name of buyer:")
    f=open("purchase.dat","rb")
    u=open("pur2.dat","ab")
    name=input("item name:")
    qty=int(input("quantity:"))
    try:
        while True:
            obj=pickle.load(f)
            if obj[1]==name:
                obj[2]=obj[2]-qty
                price=obj[3]
                tax=obj[4]
                ct=1
                pickle.dump(obj,u)
                profit=int(input(" profit percentage"))
                fp=qty*((price)+(price)*(profit/100))
                fpt=(fp)+(fp)*(tax/100)
                pp=fpt-(price*qty)
                print("final price:",fpt,"profit:",pp)
                l=[buyer,name,fpt,pp,profit,tdate]
                print(l)
                salefile(l)
                
            else:
                pickle.dump(obj,u)
                
            
             
    except EOFError:
        u.close()
        f.close()
        copy()
        up=open("pur2.dat","wb")
        up.close()
        if ct==0:
            print("no such value")
        pass
            
   
    ask("sa")
    
#storing data in file
def salefile(n):
    s=open("sale.dat","ab")
    pickle.dump(n,s)
    s.close()
    


    
#contoller.....
def ask(n):
    ask=input("may proced:")
    if ask in ["y","yes","y"] :
        if n=="p" :
            purchase()
        elif n=="s":
            search()
        elif n=="sa":
            sale()
        elif n=="d":
            delete()
        else:
            print("no such parameter")
    else:
        con=input("do you want to continue:")
        if con in ["y","yes","y"] :
            intro()
        else:
            load()
            print("thanks")
    
#for searching
def search():
    ct=0
    di={"idno":0,"name":1,"qty":2,"price":3,"tax":4}
    se=input("enter the parameter on which to be searched:")
    con=input("enter the condition 1:equality 2:less than 3:greater than ::")
    if se in di.keys() and con in ['1','2','3']:
        va=input(" this value:")
        f=open("purchase.dat","rb")
        
        try:
            while True:
                obj=pickle.load(f)
                d2={"idno":obj[0],"name":obj[1],"qty":obj[2],"price":obj[3],"tax":obj[4]}
                if se!="name":
                    va2=int(va)
                else:
                    va2=va
                
                if con=="3":
                    if obj[di[se]]>va2:
                        print(d2,"3")
                        ct+=1
                elif con=="1":
                    if obj[di[se]]==va2:
                        print(d2,"1")
                        ct+=1
                elif con=="2":
                    if obj[di[se]]<va2:
                        print(d2,"2")
                        ct+=1
            
                    
            
        except EOFError:
            if ct==0:
                print("no such value")
            pass

        f.close()
        ask('s')
    else:
        print("invalid parameter")
#for showing whole transaction at end of program
def load():
    f=open("purchase.dat","rb")
    print("showing purchasae:")
    try:
        while True:
            obj=pickle.load(f)
            print(obj)
    
    except EOFError:
        pass
        f.close()
    s=open("sale.dat","rb")
    print("showing sale")
    try:
        while True:
            obj=pickle.load(s)
            d3={"buyer":obj[0],"name":obj[1],"sale price":obj[2],"profit":obj[3],"percentage":obj[4],"date":obj[5]}
            print(d3)
    
    except EOFError:
        pass
        s.close()





#for updation of data in main file    
def copy():
    f=open("purchase.dat","wb")
    u=open("pur2.dat","rb")
    try:
        while True:
            obj=pickle.load(u)
            pickle.dump(obj,f)
    except EOFError:
        pass
    f.close()
    u.close()


#intoduction
def intro():
    typ=input("press p:purchase,s:search,sa:sale,d:delete:")
    ask(typ)    
intro()

