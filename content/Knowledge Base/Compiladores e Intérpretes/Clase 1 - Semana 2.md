![[Pasted image 20241216154417.png]]
![[Pasted image 20241216154430.png]]
![[Pasted image 20241216154442.png]]
http://cgosorio.es/Seshat/thompson?expr=a.(a%7Cb.a)*%7Cc*.a

![[Pasted image 20241216154625.png]]
![[Pasted image 20241216154651.png]]

=== NFA 1: a(a|b)a(a|b) ===
aceptadas:
- aaaa
- aaab
- abaa
- abab

rechazadas:
- aaba
- abb
- baaa
- abba

=== NFA 2: abc | a(b|d)d ===
aceptadas:
- abc
- abd
- add

rechazadas:
- ab
- abdd
- adc
- aaa
