# Tema 3 

## Informații temă
**Deadline**: **24 mai 2020** 


Predarea soluției se va face într-un repository de github în două feluri: 

1) adăugați sursele modificate sau folosite în directorul `src`; 
2) modificați template-ul [Rezolvare.md](https://github.com/senisioi/tema3/blob/master/Rezolvare.md) și completați raportul cu cerințele de acolo.



Pentru a vă înscrie folosiți acest link: [lolo](https://lolo)
Tema se va rezolva în echipe de maxim două persoane iar punctajul temei este 10% din nota finală.
Veți fi evaluați individual în funcție de commit-uri în repository prin `git blame` și `git-quick-stats -a`. Doar utilizatorii care apar cu modificări în repository vor fi punctați (în funcție de modificările pe care le fac).

### Barem
1. CRC HTTP service - 5%
2. Protocoale de routare - 5%

https://github.com/senisioi/computer-networks/tree/2020/curs#forwarding

## Cerințe temă 

### 1. CRC HTTP service
Folositi template-ul [crc_api.py](https://github.com/senisioi/tema4/blob/master/src/crc_api.py) pentru a crea un serviciu HTTP cu o metoda [POST](https://www.w3schools.com/tags/ref_httpmethods.asp) care are urmatoarea specificație:

- primește un sir de octeți ca date
- primii 4 octeți reprezintă polinomul pe maximum 32 de biți, unsigned long ('!L').
- restul de octeți reprezintă mesajul pentru care trebuie calculat CRC după polinomul dat
- returnează (!returnează, nu printează) calculul CRC după polinomul dat

Dacă în scriptul simple_flask.py din capitolul2 evaluam conținutul json din request, aici vom extrage direct octeții din `request.data`.

Testați aplicația folosind requests, header-ul trebuie să fie: `{'Content-Type': 'application/octet-stream'}` iar datele trebuie împachetate cu struct.pack('...')
```python
header = ... # vezi in cerinta
data = struct.pack('...', ...) # primii 32 de biti sunt unsigned long restul sunt octetii care reprezinta mesajul
url = 'http://ec2-21-12-21-12.compute-1.amazonaws.com' # link-ul catre serverul AWS
response = requests.post(url, headers=header, data=data)
print (response.content)
crc = struct.unpack('...', response.content) # raspunsul trebuie si el despachetat in funcie de cum a fost calculat
print ('CRC calculat: ', crc)
```

##### 1.1 Cum executăm scriptul pe serverul AWS
Puteți rula scriptul direct pe server folosind tmux, dar mai întâi trebuie să instalați pip și librăria flask pe server:
```bash
sudo apt-get update
sudo apt install python3-pip
pip3 install flask --user
tmux 
python3 tema4-repo/src/crc_api.py
# Apas Ctrl+b si apoi d pentru a ma detasa de sesiune
```
Vezi [aici tmux cheatsheet](https://tmuxcheatsheet.com/) sau câteva exemple mai jos: 
```bash
tmux ls - listez toate sesiunile
tmux - creez o noua sesiune cu indicele 0,1,2...
tmux attach -t 0 - ma atasez la sesiunea 0
Ctrl + b apoi apas d - face detach de sesiune
Ctrl + b apoi apas s - face switch de sesiune
Ctrl + b apoi apas [ - pentru scroll up
copy to clipboard - tin apasat Shift, selectez, click dreapta copy
```

##### 1.2 Cum executăm scriptul din docker pe serverul AWS
Clonăm repository cu tema4 pe care l-am primit. Modificăm [docker-compose.yml](https://github.com/senisioi/tema4/blob/master/docker-compose.yml) pentru containerul `crc_http` ca portul de pe host să fie 80.
Executăm `docker-compose up -d` apoi `docker-compose exec crc_http bash -c 'python3 /elocal/src/simple_flask.py'`



### 2.
