update all  indecies inthe provided list
recursion tree, recursion diagram and everything visual

def f(l,x,index,ans):
base case if  len(l)==index retrun 

alternate these two bellow for the order change
if l[index]==x append index in the ans
recursive call with index+1 and the ans and l and x


```py 

def UpdateAllIndicesOfAnElementInProvidedList(l1,x,index,ansList):
    if(len(l1)==index):
        return
    
    if(l1[index]==x):
        ansList.append(index)

    UpdateAllIndicesOfAnElementInProvidedList(l1,x,index+1,ansList)



ansList=[]
UpdateAllIndicesOfAnElementInProvidedList([3,2,5,2,8,2,1],10,0,ansList)

print(ansList)
 
```