# Sort
### 이차원 문자열 배열 정렬
* 네번째 아이템으로 정렬하는 경우, 문자열이 `c` 또는 `C`로 시작 한다면 앞쪽에 위치
* (참고)compareTo()의 함수에서
    * 앞에 값(o1)과 뒤의 값(o2)을 비교해서 리턴값을 양수로 주면 값을 바꿈 -> 오름차순  
    * 앞에 값(o1)과 뒤의 값(o2)을 비교해서 리턴값을 음수로 주면 값을 바꾸지 않음 -> 내림차순
```java
Collections.sort(items, new Comparator<List<String>>() {
            @Override
            public int compare(List<String> o1, List<String> o2) {
            int result;
            if (orderDirection == 0) {
                if(orderBy == 3) {
                    // c 또는 C로 시작하는 지 여부
                    boolean o1p = o1.get(orderBy).matches("[c|C].*");
                    boolean o2p = o2.get(orderBy).matches("[c|C].*");
                    // o1, o2 모두 c 또는 C로 시작하는 경우
                    if(o1p && o2p) {
                        result = o1.get(orderBy).compareTo(o2.get(orderBy));
                    } else if(o1p){ // o1p만 c 또는 C로 시작하는 경우
                        result = -1;
                    } else if(o2p){ // o2p만 c 또는 C로 시작하는 경우
                        result = 1;
                    } else { // 두 문자열 모두 c 또는 C로 시작하지 않는 경우
                        result = o1.get(orderBy).compareTo(o2.get(orderBy));
                    }
                    // System.out.println("result: "+result+" ## "+items);
                    
                }else {
                    try {
                        result =
                            Integer.compare(Integer.parseInt(o1.get(orderBy)),
                                Integer.parseInt(o2.get(orderBy)));
                    } catch (NumberFormatException e) {
                        result = o1.get(orderBy).compareTo(o2.get(orderBy));
                    }
                }
            } else {
                try {
                result =
                    Integer.compare(Integer.parseInt(o2.get(orderBy)),
                        Integer.parseInt(o1.get(orderBy)));
                } catch (NumberFormatException e) {
                    result = o2.get(orderBy).compareTo(o1.get(orderBy));
                }
            }
            return result;
            }
            // int extractInt(String s) {
            //     String num = s.replaceAll("\\d", "");
            //     // 숫자가 존재하지 않는 경우, 0을 리턴
            //     return num.isEmpty() ? 0: Integer.parseInt(num);
            // }
        });
```