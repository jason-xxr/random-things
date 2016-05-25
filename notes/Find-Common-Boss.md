
Francis Yang [10:41 PM] 
added and commented on a Java snippet: Find common boss 
```java
public void solveOnline(Scanner sc) {
​
        Map<String, String> map = new HashMap<String, String>();
        Set<String> tmpAncestors = new HashSet<String>();
        int n = sc.nextInt();
        while (n-- > 0) {
            String str1 = sc.next();
            String str2 = sc.next();
            map.put(str2, str1);
        }
        int m = sc.nextInt();
        while (m-- > 0) {
            String tStr1 = sc.next();
            String tStr2 = sc.next();
            tmpAncestors.clear();
            tmpAncestors.add(tStr1);
            while (map.get(tStr1) != null) {
                String tmp = map.get(tStr1);
                tmpAncestors.add(tmp);
                tStr1 = tmp;
            }
            while (true) {
                if (tmpAncestors.contains(tStr2)) {
                    System.out.println(tStr2);
                    break;
                }
                tStr2 = map.get(tStr2);
                if (tStr2 == null) {
                    System.out.println("-1");
                    break;
                }
            }
        }
        sc.close();
    }
```
1 Comment Collapse
自己写的， 30行code

Francis Yang [10:54 PM] 
这个是应对online版本的。。如果要求offline 大量的query，需要用union find ＋ DFS 剪枝

