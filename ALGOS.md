**BINARY SEARCH (VYHLEDÁVÁNÍ PŮLENÍM INTERVALU)**
------------------------------------------------------------------------------------------------------
- Nutná seřazená sada prvků 
- Dělí pole na dvě poloviny (proto binární)
- Rozděl a panuj
- Lze využít iterativní a rekurzivní způsob
- Průběh:
	1) Podíváme se na hodnotu uprostřed pole (délka/2)
	2) Pokud je hledaný prvek menší/větší než prostředek pole => omezíme se na levou/pravou část pole
	3) Na vybrannou polovinu aplikujeme stejné pravidlo (1/4 původního pole) 
		-  n -> n/2 -> n/4 -> ... -> 1
	4) Opakujeme => stále se přibližujeme k hledánemuu prvku
	5) Končíme ve chvíli, kdy je v prostředku prvek, který hledáme nebo jakmile už nemáme co dělit (tzn. prvek tam není)
- Složitost: **O(log n)** (n = počet prvků v poli)
- Ideální na vyhledávání v poli
- Nevhodné pokud chceme pole měnit
- ![[Pasted image 20220818120317.png]]
- Př.:
```python

def binarysearch(arr,left,right,target):
	while left <= right:
		mid = left + (right - left) // 2
		
		if arr[mid] == target:   #Je x v mid?
			return mid
			
		elif arr[mid] < target:  #Pokud je X větší, ignoruj levou půlku
			left = mid + 1
			
		else:
			right = mid - 1      #Pokud je X menší, ignoruj pravou půlku
			
	return -1                  #Pokud dojdeme až sem, element není v poli, vrátí -1

arr = [2, 3, 4, 10, 40]
target = 10

result = binarysearch(arr, 0, len(arr)-1, target)
```
**BREADTH-FIRST SEARCH  (PROHLEDÁVÁNÍ DO ŠÍŘKY)** 
-----------------------------------------------------------------------------------------------------
- Grafový algoritmus
- Postupně prochází všechny vrcholy v dané komponentě souvislosti (maximální souvislý podgraf)
- Nejprve projde všechny sousedy startovního vrcholu, poté soused sousedů => dokud neprojde celou komponentu souvislosti
- Poznamenává si předchůdce jednotlivých uzlů 
- Veškeří následovníci uzlu vkladány do FIFO fronty
- V typických implementacích:
	- Uzly, které nebyly objeveny = FRESH
	- Uzly, které se dostávají do fronty, které jsou vyšetřovány jejich násloedníky = OPEN
	- Uzly, které byly z fronty vybrány a už se s nimi nebude pracovat = CLOSE
Průběh:
	 1) Začneme procházet graf z libovolného uzlu
	 2) Uzel zpracujeme a při objevení nenalezených potomků uložíme do fronty
	 3) Pro každý vrchol si budeme pamatovat značku, zda jsme ho navštivili
	 4) Postupně zpracováváme a postupujeme, dokud fronta není prázdná
	 (Díky ukládání do fronty => dochází k procházení grafu po vlnách, uzly zpracovávány v pořadí daném vzdálenosti od kořene)
	 5) Výstup algoritmu = BF-strom (strom nejkratších cest z daného uzlu)
- Složitost: **O(v + e)** (v = nodes; e = edges)
- Využití: Hledání nejkratší cesty v grafu/zda je graf zcela propojen
- ![[Pasted image 20220818140500.png]]
- Př.:
```python
graph = {
	'5' : ['3','7'],
	'3' : ['2','4'],
	'7' : ['8'],
	'2' : [],
	'4' : ['8'],
	'8' : ['3','7'],
}

visited = [] 
queue - []

def bfs(visited, graph, node):
	visited.append(node)
	queue.append(node)
	
	while queue:
		m = queue.pop(0)
		print(m, end = " ")
		
		for neighbour in graph[m]:
			if neighbour not in visited:
				visited.append(neighbour)
				queue.append(neighbour)
				
bfs(visited,graph,'5')
#Výsledek = 5 3 7 2 4 8

bfs(visited,graph,'3')
#Výsledek = 3 2 4 8
```
**DEPTH-FIRST SEARCH  (PROHLEDÁVÁNÍ DO HLOUBKY)**
----------------------------------------------------------------------------------------------------------
