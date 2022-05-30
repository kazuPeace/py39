import csv
import math
import random
import numpy as np

def main():
    
    #テーマパークのデータファイルを開く
    try:
        f = open('parkTraffic.csv', 'r')
        dataReader = csv.reader(f)
        dataList=[]#配列の初期化
        for row in dataReader:
            dataList.append(row)#データを配列に代入
        f.close()
    except FileNotFoundError as e:
        print('parkTraffic.csvを作成してください。')
        exit()
    num=len(dataList)#データ数
    coordinates=np.zeros((num,2))#初期値0、2列の行列
    for i in range(num):
        coordinates[i]=[dataList[i][1],dataList[i][2]]#座標だけ取り出す
    
    distances=np.zeros((num,num))#正方行列を定義
    for i in range(num):
        for j in range(num):
            bec=coordinates[i]-coordinates[j]
            distances[i][j]=np.linalg.norm(bec)#２点間の距離

    routeBinary=np.zeros((num,num))
    passed=np.zeros(num)
    #最近近傍法
    index=0
    for i in range(num-1):#出入口からアトラクションを近い順に進む
        passed[i]=index
        argsorted=np.argsort(distances[index])
        for p in passed:
            argsorted=argsorted[argsorted!=p]
        routeBinary[index][argsorted[0]]=1
        index=argsorted[0]
        
    routeBinary[index][0]=1#出入口に戻る
    print(routeBinary)
    print(np.sum(routeBinary*distances))
    
    #通常の道順の形式に変更
    route=np.zeros(num+1)
    idxFrom=0
    for i in range(len(route)):
        route[i]=idxFrom
        idxFrom=np.where(routeBinary[idxFrom]==1)[0][0]

    #2opt+SA
    temp=10000#開始温度
    endTemp=0#終了温度
    step=0.1#ステップ
    new=route.copy()#新しいルートの候補
    shortest=route.copy()
    shortestBinary=routeBinary.copy()
    while temp>endTemp:
        pointA=random.randint(1,num-1)#移動させるものをランダムに選択
        notNeighbor=np.arange(0,num)
        neighbor=[0,pointA-1,pointA]
        if pointA!=num-1:neighbor.append(pointA+1)
        notNeighbor=np.delete(notNeighbor,neighbor,None)
        pointB=random.choice(notNeighbor)
        
        
        if(pointA>pointB):
            pointA,pointB=pointB,pointA
        
        new=route.copy()
        for i in range(pointB-pointA+1):
            new[pointA+i]=route[pointB-i]

        newBinary=np.zeros((num,num))
        for i in range(num):
            newBinary[int(new[i])][int(new[i+1])]=1#バイナリデータに変換
    
        total=np.sum(routeBinary*distances)
        totalNew=np.sum(newBinary*distances)
        if total>=totalNew:#距離が短くなっていれば候補を採用
            route=new.copy()
            routeBinary=newBinary.copy()
        elif random.randrange(0,1)<math.exp((total-totalNew)/temp):#短くなくても受理率で候補を採用

            route=new.copy()
            routeBinary=newBinary.copy()
        if np.sum(shortestBinary*distances)>np.sum(routeBinary*distances):
            shortest=route.copy()
            shortestBinary=routeBinary.copy()
        temp-=step
    print(shortest)
    print(shortestBinary)
    print(np.sum(shortestBinary*distances))
    
    return 0
    
    
if __name__ == "__main__":
    main()
