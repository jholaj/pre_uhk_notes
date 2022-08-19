# **VYHLEDÁVÁCÍ ALGORITMY**

## **BINARY SEARCH (VYHLEDÁVÁNÍ PŮLENÍM INTERVALU)**
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

![](Images/Pasted%20image%2020220818120317.png)
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

## **BREADTH-FIRST SEARCH  (PROHLEDÁVÁNÍ DO ŠÍŘKY)** 
- Grafový algoritmus
- Postupně prochází všechny vrcholy v dané komponentě souvislosti (maximální souvislý podgraf)
- Nejprve projde všechny sousedy startovního vrcholu, poté soused sousedů => dokud neprojde celou komponentu souvislosti
- Poznamenává si předchůdce jednotlivých uzlů 
- Veškeří následovníci uzlu vkladány do FIFO fronty
- V typických implementacích:
	- Uzly, které nebyly objeveny = FRESH
	- Uzly, které se dostávají do fronty, které jsou vyšetřovány jejich násloedníky = OPEN
	- Uzly, které byly z fronty vybrány a už se s nimi nebude pracovat = CLOSE
- Průběh:
	 1) Začneme procházet graf z libovolného uzlu
	 2) Uzel zpracujeme a při objevení nenalezených potomků uložíme do fronty
	 3) Pro každý vrchol si budeme pamatovat značku, zda jsme ho navštivili
	 4) Postupně zpracováváme a postupujeme, dokud fronta není prázdná
	 (Díky ukládání do fronty => dochází k procházení grafu po vlnách, uzly zpracovávány v pořadí daném vzdálenosti od kořene)
	 5) Výstup algoritmu = BF-strom (strom nejkratších cest z daného uzlu)
- Složitost: **O(v + e)** (v = nodes; e = edges)
- Využití: Hledání nejkratší cesty v grafu/zda je graf zcela propojen

![](Images/Pasted%20image%2020220818140500.png)
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
queue = []

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

## **DEPTH-FIRST SEARCH  (PROHLEDÁVÁNÍ DO HLOUBKY)**
- Metoda backtrackingu
- Vždy expanduje prvního následníka každého vrcholu, pokud je ještě nenavštívil
- Pokud narazí na vrchol, z kterého nejde pokračovat (nemá návštěvník/byly navštíveny), vrací se zpět backtrackingem
- Stejně jako BFS, používá stavy vrcholů "FRESH", "OPEN", a "CLOSED"
- Během prohledávání lze přidělit časové značky (vhodné pro analýzu)
- Alogritmus vždy najde řešení, ale není optimální (pokud grafem není strom)
- Průběh:
	1) Inicializace všech uzlů grafu a nastavení jejich předchůdců
	2) Každý uzel nastaven jako "FRESH" (zatím nemá určeného přechůdce) [v poli předchůdců vše na null]
	3) Projdi všechny uzly, které jsou dosud ve stavu FRESH metodou DFS
	4) Metoda realizuje vlastni prohledávání do hloubky
	5) Stav uzlu u, pro který byla metoda volána, nastaven na "OPEN"
	6) Poté se pro každého následníka (v) uzlu zjistí, zda je ještě ve stavu "FRESH"
	7) Ano =>Zapíše se pro něj do pole [p] hodnota předchůdce a zavolá se pro něj rekurzivně DFS
	8) Ne => Uzel se prohlásí za uzavřený "CLOSED"
-  Složitost: **O(v + e)** (v = nodes; e = edges)
- Využití: topologické uspořádání uzlů grafu, nalezení silných komponent grafu, acykličnost grafu

![](Images/Pasted%20image%2020220819082858.png)
- Př.:
```python
graph = {
	'0' : set(['1','2']),
	'1' : set(['0','3','4']),
	'2' : set(['0']),
	'3' : set(['1']),
	'4' : set(['2','3']),
}

def dfs(graph, start, visited=None):
	if visited is None:
		visited = set()
	visited.add(start)

	print(start)

	for next in graph[start] - visited:
		dfs(graph, next, visited)
	return visited
				
dfs(graph,'0')
#Výsledek = 0 1 4 2 2 3 3 2

dfs(graph,'1')
#Výsledek = 1 0 2 4 3 3
```

## **ROZDÍLY MEZI BFS A DFS** 
| Parametr         | BFS                                                                                                                               | DFS                                                                                                                                               |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| Datové typy      | Používá frontu (FIFO)                                                                                                             | Používá zásobník (LIFO)                                                                                                                           |
| Definice         | Metoda procházení, při které nejprve procházíme všechny uzly na stejné úrovni a teprve poté přecházíme na další úroveň            | Procházení začíná v kořenovém uzlu a postupuje přes uzly tak daleko, jak je to jen možné, dokud nedosáhneme uzlu bez nenavštívených okolních uzlů |
| Technika         | Lze použít k nalezení jediné zdrojové nejkratší cesty v neváženém grafu, protože v BFS dosáhneme vrcholu s minimálním počtem hran | S DFS můžeme projít více hran, abychom dosáhli vrcholu                                                                                            |
| Koncepční rozdíl | BFS vytváří strom po jednotlivých úrovních                                                                                        | DFS sestavuje strom po podstromech                                                                                                                |
| Vhodnější        | Pro hledání vrcholů blíže k danému zdroji                                                                                         | V případě, že existují řešení vzdálená od zdroje                                                                                                  |
| Paměť            | Vyžaduje více paměti                                                                                                              | Vyžaduje méně paměti                                                                                                                              |
| Rychlost         | Pomalý s porovnáním DFS                                                                                                           | Rychlý oproti BFS                                                                                                                                 |

![](Images/Pasted%20image%2020220819101709.png)
