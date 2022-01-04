# Documents Analysis - Exiftool

This challenge is to identify which file in a specific directory was modified in the month before christmas 

I ran exiftool recursively and searched for any anomalies in the modified by field:
```
exiftool -r ./ | grep Modified
```

![image](https://user-images.githubusercontent.com/54121441/148133619-c0d650df-4b19-46f0-b5aa-afb70e151c66.png)

I also exported the results to analyze further:

```
exiftool -r ./ > results.txt
```



Next, I used grep and the Exiftool output to see what file was modified by jack:
```
grep -A 40 Jack ./results.txt
```

![image](https://user-images.githubusercontent.com/54121441/148133641-5de73267-de74-4954-9263-0ed9c0afb85a.png)

Now, we have the name of the compromised file: **2021-12-21.docx**
