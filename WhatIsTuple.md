A tuple is a sequence of immutable(無法改變) Python objects. Tuples are sequences, just like lists. The differences between tuples and lists are, the tuples cannot be changed unlike lists and tuples use parentheses, whereas lists use square brackets.

tuple是無法改變內容的，而list可以

建立tuple是相當簡單的，只需要使用comma-separated value
Creating a tuple is as simple as putting different comma-separated values. Optionally you can put these comma-separated values between parentheses also. For example −

``` python
tup1 = ('physics', 'chemistry', 1997, 2000);
tup2 = (1, 2, 3, 4, 5 );
tup3 = "a", "b", "c", "d"; #無使用括弧也行
```

## 你也可以建構一個無內容的tuple

``` python
tup1 = ();
```

## 即使tuple內只有一個值，仍然需要加上一個comma

``` python
tup1 = (50,);
```

## 如何存取tuple的數值
``` python
#!/usr/bin/python

tup1 = ('physics', 'chemistry', 1997, 2000);
tup2 = (1, 2, 3, 4, 5, 6, 7 );

print "tup1[0]: ", tup1[0]
print "tup2[1:5]: ", tup2[1:5]
```
