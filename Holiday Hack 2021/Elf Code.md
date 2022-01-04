## The Elf Code





LEVEL 5

```
import elf, munchkins, levers, lollipops, yeeters, pits
lever0, lever1, lever2, lever3, lever4 = levers.get()
elf.moveLeft(2)
lever4.pull(lever4.data() + " concatenate")
# Don't forget to move to the KringleCon entrance!
elf.moveUp(2)

lever3.pull(not (lever3.data()))
elf.moveUp(2)

lever2.pull(lever2.data() + 1)
elf.moveUp(2)

list1=lever1.data()
list1.append(1)
lever1.pull(list1)
elf.moveUp(2)

dict1 = lever0.data()
dict1["strkey"] = ("strvalue")
lever0.pull(dict1)
elf.moveUp(2)
elf.moveUp(2)
```

LEVEL 6

```
import elf, munchkins, levers, lollipops, yeeters, pits
# Fix/Complete the below code
lever = levers.get(0)
data = lever.data()
if type(data) == bool:
    answer = not data
elif type(data) == int:
    answer = data * 2 
elif type(data) == list:
    answer = data
    for i in answer:
        i = i + 1
elif type(data) == str:
    answer = data + data
elif type(data) == dict:
    data["a"] = data.get("a") + 1
    answer = data
elf.moveTo(lever.position)
#lever.something
lever.pull(answer)
elf.moveUp(2)
```

LEVEL 8

```
import elf, munchkins, levers, lollipops, yeeters, pits
all_lollipops = lollipops.get()
lever0 = levers.get(0)
for lollipop in all_lollipops:
    elf.moveTo(lollipop.position)
elf.moveTo(lever0.position)
array = lever0.data()
array.insert(0,"munchkins rule")
lever0.pull(array)
elf.moveDown(3)
elf.moveLeft(6)
elf.moveUp(3)
```
