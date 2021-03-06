# Bazele Git #

Dacă doriți să citiți un singur capitol inainte de a începe lucrul cu Git, acesta este cel recomandat. Acest capitol acoperă toate comenzile de bază cu care puteți face majoritatea operațiilor pe care le veți putea efectua eventual folosind Git. Pănă la sfărșitul capitolului, ar trebui să puteți să configurați și să inițializați un repository, să începeți și să vă opriți să urmăriți anumite fișiere, și să puneți în așteptare (stage [en]) și să faceți commit la schimbările făcute. Vă vom arăta de asemenea cum puteți să setați Git ca să ignore anumite fișiere sau tipuri de fișiere, cum să reparați greșelile simplu și rapid, cum să navigați prin istoria proiectului dumneavoastră și cum să vedeți schimbările făcute între commit-uri, și cum să trimiteți și să preluați (push/pull [en]) din repository-uri din rețea.

## Preluarea Unui Repository Git ##

Puteți prelua un proiect Git folosind una din cele două abordări. Prima metodă preia un proiect existent sau un director și îl importă în Git. A doua metodă clonează un repository Git existent de la o adresă specificată.

### Inițializarea Unui Repository într-un Director Existent ###

Dacă doriți să începeți să urmăriți un proiect existent în Git, aveți nevoie să mergeți în directorul proiectului și să scrieți

	$ git init

Această comandă creează un nou subdirector denumit .git care conține toate fișierele necesare repository-ului — un schelet al unui repository Git repository. În acest punctm, nimic din proiectul dumneavoastră nu este încă urmărit. (Vedeți Capitolul 9 pentru mai multe informații despre ce fișiere sunt conținute în directorul `.git` pe care tocmai l-ați creat.)

Dacă doriți să începeți să adăugați în sistemul de control al versiunilor fișiere deja prezente (spre diferență de un director gol), ar trebui probabil să începeți să urmăriți acele fișîere și să aveți un commit inițial. Puteți face asta cu câteva comenzi git add care specifică fișierele care doriți să fie urmărite, urmate de o comandă commit:

	$ git add *.c
	$ git add README
	$ git commit -m 'initial project version'

Vom reveni la ce fac aceste comenzi în doar câteva momente. În acest punct, aveți un repository Git cu câteva fișiere urmărite și un commit inițial.

### Clonarea Unui Repository Existent ###
Dacă doriți să preluați o copie a unui repository Git existent — de exemplu, un proiect la care ați dori să contribuiți — comanda de care aveți nevoie este git clone. Dacă sunteți familiarizați deja cu alte sisteme VCS cum ar fi Subversion, veți observa că denumirea comenzii este clonare (clone [en]) și nu checkout. Aceasta este o diferență importantă — Git primește o copie a tuturor datelor pe care le deține serverul. Fiecare versiune a fiecărui fișier pentru întreaga istorie a proiectului este preluată atunci când executați `git clone`. De fapt, dacă discul serverului se defectează, puteți folosi oricare din clonele aflate pe orice client pentru a reinițializa serverul în starea în care era atunci când ați făcut clona (puteți totuși pierde anumite setări legate de server, dar toate datele versionate ar fi la locul lor — vedeți Capitolul 4 pentru mai multe detalii). 

Puteți clona un repository cu `git clone [url]`. De exemplu, dacă doriți să clonați repository-ul Git pentru Ruby denumit Grit, puteți face asta în următorul mod:

	$ git clone git://github.com/schacon/grit.git

Această comandă creează un director denumit "grit", inițializează un director `.git` în interiorul acestuia, preia toate datele pentru acel repository, și preia și o copie a ultimei versiuni disponibile. Dacă accesați noul director `grit`, veți putea observa că acesta conține fișierele proiectului, gata de a fi utilizate sau modificate. Dacă doriți să clonați repository-ul într-un director cu alt nume decât grit, puteți specifica asta folosind următoarea opțiune în linia de comandă:

	$ git clone git://github.com/schacon/grit.git mygrit

Această comandă face același lucru ca și comanda prezentată anterior, dar directorul destinație este denumit mygrit acum.

Git are un număr de diverse protocoale de transfer pe care le puteți folosi. În exemplul anterior folosim protocolul `git://`, dar puteți de asemenea folosi și `http(s)://` sau `user@server:/path.git`, care utilizează SSH ca și protocol pentru transferul fișierelor. Capitolul 4 va introduce toate opțiunile disponibile date de server pentru a putea accesa repository-ul dumneavoastră Git cât și argumente pro și contra pentru fiecare din cazuri.

## Înregistrarea Schimbărilor în Repository ##

Acum aveți un repository Git bona fide și un checkout (preluare [ro]) recentă a fișierelor pentru acel proiect. Trebuie să faceți niște schimbări și să comiteți instantanee ale acelor schimbări în repository de fiecare dată când proiectul ajunge într-o stare pe care doriți să o păstrați.

Țineți minte că fiecare fișier din directorul curent poate fi într-una din cele două stări: urmărit sau ne-urmărit (tracked/untracked [en]). Fișierele urmărite sunt fișiere care erau prezente și în ultimul snapshot; acestea pot fi neatinse, modificate, sau staged. Fișierele ne-urmărite sunt toate celelalte — orice fișiere din directorul curent care nu erau în ultimul snapshot și nu sunt prezente în zona de staging. Atunci când clonați pentru prima data un repository, toate fișierele dumneavoastră vor fi urmărite și neatinse deoarece tocmai le-ați preluat și nu ați editat niciunul din ele. 

Pe măsură ce editați fișiere, Git le vede ca și modificate, deoarece le-ați modicat de la ultimul comit. Pregătiți aceste fișiere modificate și apoi comiteți toate schimbările pregătite (staged [en]), iar apoi ciclul se repetă. Acest ciclu este prezentat în Figura 2-1.

Insert 18333fig0201.png
Figura 2-1. Ciclul de viață al stării fișierelor dumneavoastră.

### Verificarea Stării Fișierelor Dumneavoastră ###

Unealta principală pe care o folosiți pentru a determina care fișiere sunt prezente și starea lor este git status. Dacă rulați această comandă imediat după git clone, ar trebui să obțineți ceva similar cu:

	$ git status
	# On branch master
	nothing to commit (working directory clean)

Aceasta înseamnă că aveți un director preluat curat — sau în alte cuvinte, că nu există fișiere urmărite care sunt modificate. Git de asemenea nu poate observa fișiere ne-urmărite, sau ele ar fi prezente aici. În cele din urmă, comanda ne spune pe care ramură (branch [en]) a proiectului ne aflăm. Pentru moment, aceasta este întotdeauna master, care este cea implicită; încă nu trebuie să ne facem griji pentru acest aspect încă. În următorul capitol vom avea în vedere ramurile și referințele în detaliu.

Să presupunem acum că dorim să adăugăm un fișier la proiect, un simplu fișier README. Dacă fișierul nu a existat înainte, și rulăm `git status`, vom putea observa fișierele ne-urmărite astfel:

	$ vim README
	$ git status
	# On branch master
	# Untracked files:
	#   (use "git add <file>..." to include in what will be committed)
	#
	#	README
	nothing added to commit but untracked files present (use "git add" to track)

Puteți observa că noul fișier README nu este urmărit, deoarece este în categoria "Untracked files" (fișiere ne-urmărite [ro]) afișat de comanda status. Aceasta înseamnă că Git observă un fișier care nu era prezent în instantaneul trecut (commit); Git nu va adăuga acest fișier în mod automat la cele urmărite până în momentul în care nu îi indicați explicit acest fapt. Acest comportament este folosit pentru a nu include în mod accidental fișiere binare generate sau alte fișiere pe care nu doreați să le urmăriți. Acum pentru că dorim să includem README, vom începe să adăugăm și acest fișier la lista celor urmărite. 

### Urmărirea Noilor Fișiere ###

Pentru a începe urmărirea unui nou fișier, vom folosi comanda `git add`. Pentru a adăuga fișierul README la cele urmărite, vom rula următoarea comandă:

	$ git add README

Dacă executăm iarăși comanda status, putem vedea faptul că fișierul README este acum pregătit și urmărit:

	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#	new file:   README
	#

Putem determina faptul că este pregătit pentru că este în categoria “Changes to be committed”. Dacă comiteți în acest moment, versiunea fișierului la momentul când ați efectuat git add este ceea va fi prezent în instantaneul istoriei. Poate vă amintiți că în momentul când ați efectuat git init, am executat apoi comanda git add (fișiere) — folosită pentru a începe să urmărim fișiere din directorul dumneavoastră. Comanda git add preia o cale de fișier fie pentru un fișier fie pentru un director; dacă este un director, comandă adaugă toate fișierele din acel director în mod recursiv.

### Pregătirea Fișierelor Modificate ###

Să schimbăm un fișiere care era deja urmărit. Dacă schimbați un fișier urmărit anterior denumit `benchmarks.rb` și apoi rulați `status`, vom obține ceva similar cu:

	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#	new file:   README
	#
	# Changed but not updated:
	#   (use "git add <file>..." to update what will be committed)
	#
	#	modified:   benchmarks.rb
	#

Se pare că fișierul benchmarks.rb apare în secțiunea “Changed but not updated” (schimbat dar ne-actualizat) — ceea ce înseamnă că un fișier care este urmărit a fost modificat în directorul de lucru dar nu a fost încă staged. Pentru a-l pregăti, puteți executa comanda `git add` (este o comandă cu mai multe scopuri — o folosim pentru a începe să urmărim fișiere noi, pentru a pregăti fișiere ce urmează să fie comise, și pentru a face alte lucruri cum ar fi să marcăm problemele date de conflice de merging (fuzionare [ro]) ca și rezolvate). Să executăm `git add` acum pentru a pregăti fișierul benchmarks.rb, și apoi să executăm `git status` iarăși:

	$ git add benchmarks.rb
	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#	new file:   README
	#	modified:   benchmarks.rb
	#

Ambele fișiere sunt pregătite și vor intra în următorul comit. În acest punct, să presupunem că vă amintiți faptul că doreați să mai faceți o mică schimbare în benchmarks.rb înainte de a-l comite. Editați fișierul cu adiția dorită, și acum sunteți gata să vă incheiați treaba. Oricum, să executăm `git status` o ultimă dată:

	$ vim benchmarks.rb
	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#	new file:   README
	#	modified:   benchmarks.rb
	#
	# Changed but not updated:
	#   (use "git add <file>..." to update what will be committed)
	#
	#	modified:   benchmarks.rb
	#

Dar ce se întâmplă? Acum benchmarks.rb este listat atât ca pregătit cât și ne-pregătit. Cum este una ca asta posibil? Se pare că Git pregătește un fișier exact ca atunci când executăm comand git add. Daca comiteți acum, versiunea fișierului benchmarks.rb este cea din momentul când ați rulat git add și este cea care se va trimite în acest instantaneu, și nu este versiunea fișierului modificat în prezent când executați git commit. Dacă modicați un fișier după ce executați `git add`, trebuie să rulați iarăși `git add` pentru a pregăti ultima versiune a fișierului:

	$ git add benchmarks.rb
	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#	new file:   README
	#	modified:   benchmarks.rb
	#

### Ignorarea Fișierelor ###

De multe ori, veți avea o clasă de fișiere pe care nu doriți ca Git să le adauge automat sau să le marcheze ca fiind ne-urmărite. Acestea sunt de obicei fișiere generate cum ar fi log-uri sau fișiere binare produse de sistemul de construcție al aplicației. În aceste cazuri, puteți crea un model de listare care să le ignore denumit .gitignore. Iată un exemplu de fișier .gitignore:

	$ cat .gitignore
	*.[oa]
	*~

Prima linie informează Git să ignore orice fișiere care se termină în .o sau .a — fișiere obiect sau arhivă care pot fi produse după construirea proiectului dumneavoastră. A doua linie îi spune lui Git să ignore toate fișierele care se termină cu tilda (`~`), caracter folosit de multe editoare text cum ar fi Emacs pentru a marca fișierele temporare. Puteți de asemenea include log, tmp sau directorul pid; documentație generată automat; ș.a.m.d. Configurarea unui fișier .gitignore înainte de începerea proiectului este de obicei o idee bună astfel ca să nu adăugați accidental fișiere pe care nu le doriți prezente în repository-ul Git.

Regulile pentru modelele pe care le puteți utiliza în fișierul .gitignore sunt următoarele:

*	Linii goale sau linii care încep cu # sunt ignorate.
*	Caracterele standard pentru identificarea fișierelor (glob [en]) funcționează.
*	Puteți termina modelele cu slash (`/`) pentru a specifica un director.
*	Puteți nega un model dacă începeți cu semn de exclamare (`!`).

Modelele standard pentru identificarea fișierelor sunt un model de expresii regulare folosite de linia de comandă. Unul sau mai multe steluțe (`*`) corespunde la zero sau mai multe caractere; `[abc]` corespunde oricărui caracter din paranteze (în acest caz a, b, sau c); un semn de întrebare (`?`) corespunde unui singur caracter; și paranteze în jurul caracterelor separate de liniuță(`[0-9]`) corespund oricărui caracter dintre paranteze (în acest caz cifrele de la 0 la 9) .

Iată un alt exemplu de fișier .gitignore:

	# un comentariu - acesta este ignorat
	*.a       # fără fișiere .a
	!lib.a    # dar include lib.a, chiar dacă ingorăm fișierele .a deasupra
	/TODO     # ignoră doar fișierul TODO din directorul rădăcină, nu și din subdirectorul /TODO
	build/    # ignoră toate fișierele din directorul build/
	doc/*.txt # ignoră doc/notes.txt, dar nu și doc/server/arch.txt

### Vizualizarea Schimbărilor Pregătite și a Celor Nepregătite ###

Dacă comanda `git status` este prea vagă pentru dumneavoastră — doriți să știți exact ce ați schimbat, nu doar ce fișiere au fost schimbate — puteți folosi comanda `git diff`. Vom analiza `git diff` în mai multe detalii mai târziu; dar probabil veți dori să o folosiți mai des pentru a afla răspunsul la două întrebări: Ce am schimbat dar nu este încă staged? Și ce este pregătit și urmează să comitem? Chiar dacă `git status` ne răspunde la aceste întrebări într-un mod general, `git diff` ne indică exact ce linii au fost adăugate și șterse — mai exact, patch-ul.

Să presupunem că editați și pregătiți fișierul README din nou și apoi editați fișierul benchmarks.rb fără a-l pregăti. Dacă aplelați comanda `status`, vom observa ceva similar cu:

	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#	new file:   README
	#
	# Changed but not updated:
	#   (use "git add <file>..." to update what will be committed)
	#
	#	modified:   benchmarks.rb
	#

Pentru a vedea ce este schimbat dar nu încă pregătit, scrieți `git diff` fără alte argumente:

	$ git diff
	diff --git a/benchmarks.rb b/benchmarks.rb
	index 3cb747f..da65585 100644
	--- a/benchmarks.rb
	+++ b/benchmarks.rb
	@@ -36,6 +36,10 @@ def main
	           @commit.parents[0].parents[0].parents[0]
	         end

	+        run_code(x, 'commits 1') do
	+          git.commits.size
	+        end
	+
	         run_code(x, 'commits 2') do
	           log = git.commits('master', 15)
	           log.size

Această comandă compară ceea ce este prezent în directorul de lucru cu ceea ce se află în zona de pregătire. Rezultatul ne indică schimbările făcute pe care nu le-ați pregătit încă.

Dacă doriți să vedeți ceea ce este pregătit pentru a fi adăugat în următorul comit, puteți folosi `git diff --cached`. (În Git versiunile 1.6.1 și după, puteți de asemenea folosi`git diff --staged`, ceea ce ar putea fi mai simplu de amintit.) Această comandă compară schimbările pregătite cu ultimul commit:

	$ git diff --cached
	diff --git a/README b/README
	new file mode 100644
	index 0000000..03902a1
	--- /dev/null
	+++ b/README2
	@@ -0,0 +1,5 @@
	+grit
	+ by Tom Preston-Werner, Chris Wanstrath
	+ http://github.com/mojombo/grit
	+
	+Grit is a Ruby library for extracting information from a Git repository

Este important de considerat faptul că `git diff` nu ne arată toate schimbările făcute de la ultimul comit — ci doar schimbările care sunt încă nepregătite. Aceasta poate fi puțin confuz, deoarece dacă ați pregătit toate schimbările, `git diff` nu va afișa nimic.

Pentru un alt exemplu, dacă pregătiți fișierul benchmarks.rb și apoi îl editați, puteți folosi `git diff` pentru a vedea schimbările din fișier care sunt pregătite și cele care nu sunt încă pregătite:

	$ git add benchmarks.rb
	$ echo '# test line' >> benchmarks.rb
	$ git status
	# On branch master
	#
	# Changes to be committed:
	#
	#	modified:   benchmarks.rb
	#
	# Changed but not updated:
	#
	#	modified:   benchmarks.rb
	#

Acum puteți folosi `git diff` pentru a vedea ceea ce nu este încă pregătit

	$ git diff
	diff --git a/benchmarks.rb b/benchmarks.rb
	index e445e28..86b2f7c 100644
	--- a/benchmarks.rb
	+++ b/benchmarks.rb
	@@ -127,3 +127,4 @@ end
	 main()

	 ##pp Grit::GitRuby.cache_client.stats
	+# test line

și `git diff --cached` pentru a vedea ceea ce este pregătit până în prezent:

	$ git diff --cached
	diff --git a/benchmarks.rb b/benchmarks.rb
	index 3cb747f..e445e28 100644
	--- a/benchmarks.rb
	+++ b/benchmarks.rb
	@@ -36,6 +36,10 @@ def main
	          @commit.parents[0].parents[0].parents[0]
	        end

	+        run_code(x, 'commits 1') do
	+          git.commits.size
	+        end
	+
	        run_code(x, 'commits 2') do
	          log = git.commits('master', 15)
	          log.size

### Comiterea Schimbărilor Făcute ###

Acum că zona de staging este setată cum doriți, puteți comite schimbările dumneavoastră. Țineți minte că tot ceea ce nu este nepregătit — orice fișiere pe care le-ați creat sau modificat pe care nu le-ați marcat cu `git add` de la ultima editare — nu vor fi adăugate în acest comit. Ele vor rămâne ca și fișiere modificate pe disc.
În acest caz, ultima dată când ați executat `git status`, ați văzut tot ceea ce era pregătit, așa că sunteți gata să vă adăugați schimbările. Cel mai simplu mod de a comite este să scrieți `git commit`:

	$ git commit

Făcând asta lansează editorul ales. (Acesta este configurat cu variabila de mediu `$EDITOR` — de obicei vim sau emacs, cu toate ca puteți configura orice comandă doriți folosind `git config --global core.editor` după cum a fost prezentat în Capitolul 1).

Editorul afișează următorul text (acest exemplu este dintr-un ecran Vim):

	# Please enter the commit message for your changes. Lines starting
	# with '#' will be ignored, and an empty message aborts the commit.
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#       new file:   README
	#       modified:   benchmarks.rb
	~
	~
	~
	".git/COMMIT_EDITMSG" 10L, 283C

Puteți observa că mesajul implicit pentru comit conține ultimul status al comenzii `git status` ca și comentariu și o linie goală deasupra. Puteți șterge aceste comentarii și să le înlocuiți cu propriul mesaj, sau puteți să le lăsați în forma prezentă pentru a vă reaminti ceea ce ați modificat. (Pentru un reminder mai explicit a ceea ce ați modificat, puteți alege opțiunea `-v` când apelați `git commit`. Făcând asta veți adăuga diferențele introduse în editor așa că puteți vedea exact ceea ce ați modificat.) La părăsirea editorului, Git va crea comitul cu mesajul specificat în editor (cu comentariile și diferențele scoase).

Alternativ, puteți adăuga propriul mesaj intr-o singură linie odată cu comanda `commit` specificând după ea parametrul -m, similar cu:

	$ git commit -m "Story 182: Fix benchmarks for speed"
	[master]: created 463dc4f: "Fix benchmarks for speed"
	 2 files changed, 3 insertions(+), 0 deletions(-)
	 create mode 100644 README

Acum ați creat primul comit! Puteți vedea că și comitul a prezentat anumite informații despre comit: pe care ramură ați comis către (master), ce sumă de control SHA-1 are comitul (`463dc4f`), câte fișiere au fost schimbate, și statistici despre linii de cod șterse sau adăugate în comitul curent.

Țineți minte că commit menține un instantaneu pe care l-ați setat în zona de pregătire. Orice nu ați pregătit se află încă pe disc și este încă modificat; puteți adăuga un nou comit pentru a-l adăuga la istoria înregistrată a proiectului. De fiecare dată când efectuați commit, înregistrați un instantaneu al proiectului la care puteți reveni sau cu care îl puteți compara  mai târziu.

### Sărind Peste Zona de Pregătire ###

Chiar dacă poate fi incredibil de folositor în crearea comiturilor exact cum le doriți, zona de pregătire este uneori puțin mai complexă decât este necesar într-un flux obișnuit. Dacă doriți să treceți peste acest pas, Git vă oferă o simplă scurtătură. Apelând `git commit` cu opțiunea `-a` Git aduce în zona de pregătire fiecare fișier care este deja urmărit înainte de a face comit, permițându-vă să treceți peste partea cu `git add`:

	$ git status
	# On branch master
	#
	# Changed but not updated:
	#
	#	modified:   benchmarks.rb
	#
	$ git commit -a -m 'added new benchmarks'
	[master 83e38c7] added new benchmarks
	 1 files changed, 5 insertions(+), 0 deletions(-)

Putem observa cum în acest caz nu a mai fost nevoie să apelăm `git add` pentru benchmarks.rb înainte de comit.

### Ștergerea Fișierelor ###

Pentru a șterge un fișier din Git, trebuie să îl scoatem din lista fișierelor urmărite (mai exact, să îl ștergem din zona de pregătire) și apoi să comitem. Comanda `git rm` face exact asta și de asemenea șterge fișierul din directorul de lucru astfel că nu va mai apare ca și un fișier ne-urmărit pentru următoarele operații.

Dacă pur și simplu ștergeți fișierul din directorul de lucru, acesta va apare în categoria celor schimbate dar ne-comise (“Changed but not updated” (adică, _ne-urmărite_) din cadrul comenzii `git status`:

	$ rm grit.gemspec
	$ git status
	# On branch master
	#
	# Changed but not updated:
	#   (use "git add/rm <file>..." to update what will be committed)
	#
	#       deleted:    grit.gemspec
	#

Apoi, dacă apelăm `git rm`, pregătește fișierul pentru ștergere:

	$ git rm grit.gemspec
	rm 'grit.gemspec'
	$ git status
	# On branch master
	#
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#       deleted:    grit.gemspec
	#

Următoarea dată cănd comitem, fișierul va dispare și nu va mai fi urmărit. Dacă ați modificat fișierul și l-ați adăugat la index deja, trebuie să forțați ștergerea lui cu opțiunea `-f`. Această operație este necesară pentru a preveni ștergeri accidentale ale datelor care nu au fost încă înregistrate într-un instantaneu și care nu pot fi recuperate cu ajutorul lui Git. 

Un alt lucru folositor pe care veți dori să îl faceți este să păstrați fișierul în cadrul structurii de lucru dar să îl ștergeți din zona de pregătire. Sau cu alte cuvinte, veți dori să păstrați fișierul pe hard disk dar nu doriți ca Git să îl mai urmărească. Acesta lucru este util dacă ați uitat să adăugați ceva în fișierul `.gitignore` și accidental l-ați adăugat, un exemplu bun ar fi un fișier log foarte mare sau mai multe fișiere `.a` compilate. Pentru a face asta, folosiți opțiunea `--cached`:

	$ git rm --cached readme.txt

Puteți folosi această opțiune atât pentru fișiere, directoare, și glob-pattern pentru comanda `git rm`. Deci puteți face lucruri ca și

	$ git rm log/\*.log

Luați în considerare folosirea backslash (`\`) înainte de `*`. Aceasta este necesară deoarece Gitare propriul model de expandare a numelor fișierelor în plus de cel folosit de shell-ul curent. Această comandă șterge toate fișierele cu extensia `.log` din directorul `log/`. Sau, puteți face ceva asemănător:

	$ git rm \*~

Această comandă șterge toate fișierele care se termină cu `~`.

### Mutarea Fișierelor ###

Spre diferență de multe alte sisteme VCS, Git nu vă urmărește în mod explicit mișcările de fișiere. Dacă redenumiți un fișier în Git, nu vor fi stocate metadate în Git care să îl informeze despre redenumirea fișierelor. Cu toate acestea, Git este destul de inteligent pentru a realiza schimbările după întreprinderea acțiunii — vom discuta mai târziu despre detectarea mutării fișierelor puțin mai târziu.

Așadar este puțin complicat/confusing faptul că Git are o comandă `mv`. Dacă doriți să redenumiți un fișier în Git, puteți executa o comandă similară cu următoarea

	$ git mv file_from file_to

și va funcționa corect. De fapt, dacă vă veți uita la status după rularea comenzii veți observa că Git o consideră o comandă folosită pentru redenumirea unui fișier:

	$ git mv README.txt README
	$ git status
	# On branch master
	# Your branch is ahead of 'origin/master' by 1 commit.
	#
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#       renamed:    README.txt -> README
	#

Dar, aceasta este echivalentă cu a executa ceva similar cu această comandă:

	$ mv README.txt README
	$ git rm README.txt
	$ git add README

Git determină că este vorba de o redenumire în mod implicit, așa că nu va conta dacă veți redenumi un fișier în acest fel sau cu ajutorul comenzii `mv`. Singura diferența vizibilă este că `mv` reprezintă o singură comandă în locul a trei comenzi — este doar o funcție de conveniență. Mai important, puteți folosi orice utilitar doriți pentru a redenumi un fișier, și va/vom adresa comenzile add/rm mai târziu, înainte de a comite.

## Vizualizarea Istoricului Commit-urilor ##

After you have created several commits, or if you have cloned a repository with an existing commit history, you’ll probably want to look back to see what has happened. The most basic and powerful tool to do this is the `git log` command.

These examples use a very simple project called simplegit that I often use for demonstrations. To get the project, run

	git clone git://github.com/schacon/simplegit-progit.git

When you run `git log` in this project, you should get output that looks something like this:

	$ git log
	commit ca82a6dff817ec66f44342007202690a93763949
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Mon Mar 17 21:52:11 2008 -0700

	    changed the version number

	commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Sat Mar 15 16:40:33 2008 -0700

	    removed unnecessary test code

	commit a11bef06a3f659402fe7563abf99ad00de2209e6
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Sat Mar 15 10:31:28 2008 -0700

	    first commit

By default, with no arguments, `git log` lists the commits made in that repository in reverse chronological order. That is, the most recent commits show up first. As you can see, this command lists each commit with its SHA-1 checksum, the author’s name and e-mail, the date written, and the commit message.

A huge number and variety of options to the `git log` command are available to show you exactly what you’re looking for. Here, we’ll show you some of the most-used options.

One of the more helpful options is `-p`, which shows the diff introduced in each commit. You can also use `-2`, which limits the output to only the last two entries:

	$ git log -p -2
	commit ca82a6dff817ec66f44342007202690a93763949
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Mon Mar 17 21:52:11 2008 -0700

	    changed the version number

	diff --git a/Rakefile b/Rakefile
	index a874b73..8f94139 100644
	--- a/Rakefile
	+++ b/Rakefile
	@@ -5,7 +5,7 @@ require 'rake/gempackagetask'
	 spec = Gem::Specification.new do |s|
	-    s.version   =   "0.1.0"
	+    s.version   =   "0.1.1"
	     s.author    =   "Scott Chacon"

	commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Sat Mar 15 16:40:33 2008 -0700

	    removed unnecessary test code

	diff --git a/lib/simplegit.rb b/lib/simplegit.rb
	index a0a60ae..47c6340 100644
	--- a/lib/simplegit.rb
	+++ b/lib/simplegit.rb
	@@ -18,8 +18,3 @@ class SimpleGit
	     end

	 end
	-
	-if $0 == __FILE__
	-  git = SimpleGit.new
	-  puts git.show
	-end
	\ No newline at end of file

This option displays the same information but with a diff directly following each entry. This is very helpful for code review or to quickly browse what happened during a series of commits that a collaborator has added.
You can also use a series of summarizing options with `git log`. For example, if you want to see some abbreviated stats for each commit, you can use the `--stat` option:

	$ git log --stat
	commit ca82a6dff817ec66f44342007202690a93763949
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Mon Mar 17 21:52:11 2008 -0700

	    changed the version number

	 Rakefile |    2 +-
	 1 files changed, 1 insertions(+), 1 deletions(-)

	commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Sat Mar 15 16:40:33 2008 -0700

	    removed unnecessary test code

	 lib/simplegit.rb |    5 -----
	 1 files changed, 0 insertions(+), 5 deletions(-)

	commit a11bef06a3f659402fe7563abf99ad00de2209e6
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Sat Mar 15 10:31:28 2008 -0700

	    first commit

	 README           |    6 ++++++
	 Rakefile         |   23 +++++++++++++++++++++++
	 lib/simplegit.rb |   25 +++++++++++++++++++++++++
	 3 files changed, 54 insertions(+), 0 deletions(-)

As you can see, the `--stat` option prints below each commit entry a list of modified files, how many files were changed, and how many lines in those files were added and removed. It also puts a summary of the information at the end.
Another really useful option is `--pretty`. This option changes the log output to formats other than the default. A few prebuilt options are available for you to use. The oneline option prints each commit on a single line, which is useful if you’re looking at a lot of commits. In addition, the `short`, `full`, and `fuller` options show the output in roughly the same format but with less or more information, respectively:

	$ git log --pretty=oneline
	ca82a6dff817ec66f44342007202690a93763949 changed the version number
	085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7 removed unnecessary test code
	a11bef06a3f659402fe7563abf99ad00de2209e6 first commit

The most interesting option is `format`, which allows you to specify your own log output format. This is especially useful when you’re generating output for machine parsing — because you specify the format explicitly, you know it won’t change with updates to Git:

	$ git log --pretty=format:"%h - %an, %ar : %s"
	ca82a6d - Scott Chacon, 11 months ago : changed the version number
	085bb3b - Scott Chacon, 11 months ago : removed unnecessary test code
	a11bef0 - Scott Chacon, 11 months ago : first commit

Table 2-1 lists some of the more useful options that format takes.

---------------------------------------------------------
Option Description of Output
------ --------------------------------------------------
`%H`   Commit hash

`%h`   Abbreviated commit hash

`%T`   Tree hash

`%t`   Abbreviated tree hash

`%P`   Parent hashes

`%p`   Abbreviated parent hashes

`%an`  Author name

`%ae`  Author e-mail

`%ad`  Author date (format respects the `--date=` option)

`%ar`  Author date, relative

`%cn`  Committer name

`%ce`  Committer email

`%cd`  Committer date

`%cr`  Committer date, relative

`%s`   Subject
---------------------------------------------------------
Table: Format options

You may be wondering what the difference is between _author_ and _committer_. The author is the person who originally wrote the work, whereas the committer is the person who last applied the work. So, if you send in a patch to a project and one of the core members applies the patch, both of you get credit — you as the author and the core member as the committer. We’ll cover this distinction a bit more in Chapter 5.

The oneline and format options are particularly useful with another `log` option called `--graph`. This option adds a nice little ASCII graph showing your branch and merge history, which we can see our copy of the Grit project repository:

	$ git log --pretty=format:"%h %s" --graph
	* 2d3acf9 ignore errors from SIGCHLD on trap
	*  5e3ee11 Merge branch 'master' of git://github.com/dustin/grit
	|\
	| * 420eac9 Added a method for getting the current branch.
	* | 30e367c timeout code and tests
	* | 5a09431 add timeout protection to grit
	* | e1193f8 support for heads with slashes in them
	|/
	* d6016bc require time for xmlschema
	*  11d191e Merge branch 'defunkt' into local

Those are only some simple output-formatting options to `git log` — there are many more. Table 2-2 lists the options we’ve covered so far and some other common formatting options that may be useful, along with how they change the output of the log command.

------------------------------------------------------------------------------
Option            Description
----------------- ------------------------------------------------------------
`-p`              Show the patch introduced with each commit.

`--stat`          Show statistics for files modified in each commit.

`--shortstat`     Display only the changed/insertions/deletions line from the
                  `--stat` command.

`--name-only`     Show the list of files modified after the commit
                  information.

`--name-status`   Show the list of files affected with added/modified/deleted
                  information as well.

`--abbrev-commit` Show only the first few characters of the SHA-1 checksum
                  instead of all 40.

`--relative-date` Display the date in a relative format (for example,
                  “2 weeks ago”) instead of using the full date format.

`--graph`         Display an ASCII graph of the branch and merge history
                  beside the log output.

`--pretty`        Show commits in an alternate format. Options include
                  oneline, short, full, fuller, and format (where you specify
                  your own format).
------------------------------------------------------------------------------
Table: Some `git log` options

### Limiting Log Output ###

In addition to output-formatting options, git log takes a number of useful limiting options — that is, options that let you show only a subset of commits. You’ve seen one such option already — the `-2` option, which show only the last two commits. In fact, you can do `-<n>`, where `n` is any integer to show the last `n` commits. In reality, you’re unlikely to use that often, because Git by default pipes all output through a pager so you see only one page of log output at a time.

However, the time-limiting options such as `--since` and `--until` are very useful. For example, this command gets the list of commits made in the last two weeks:

	$ git log --since=2.weeks

This command works with lots of formats — you can specify a specific date (“2008-01-15”) or a relative date such as “2 years 1 day 3 minutes ago”.

You can also filter the list to commits that match some search criteria. The `--author` option allows you to filter on a specific author, and the `--grep` option lets you search for keywords in the commit messages. (Note that if you want to specify both author and grep options, you have to add `--all-match` or the command will match commits with either.)

The last really useful option to pass to `git log` as a filter is a path. If you specify a directory or file name, you can limit the log output to commits that introduced a change to those files. This is always the last option and is generally preceded by double dashes (`--`) to separate the paths from the options.

In Table 2-3 we’ll list these and a few other common options for your reference.

------------------------------------------------------------------------------
Option                Description
--------------------- --------------------------------------------------------
`-(n)`                Show only the last n commits

`--since`, `--after`  Limit the commits to those made after the specified
                      date.

`--until`, `--before` Limit the commits to those made before the specified
                      date.

`--author`            Only show commits in which the author entry matches the
                      specified string.

`--committer`         Only show commits in which the committer entry matches
                      the specified string.
------------------------------------------------------------------------------
Table: Some `git log` filter options

For example, if you want to see which commits modifying test files in the Git source code history were committed by Junio Hamano and were not merges in the month of October 2008, you can run something like this:

	$ git log --pretty="%h - %s" --author=gitster --since="2008-10-01" \
	   --before="2008-11-01" --no-merges -- t/
	5610e3b - Fix testcase failure when extended attribute
	acd3b9e - Enhance hold_lock_file_for_{update,append}()
	f563754 - demonstrate breakage of detached checkout wi
	d1a43f2 - reset --hard/read-tree --reset -u: remove un
	51a94af - Fix "checkout --track -b newbranch" on detac
	b0ad11e - pull: allow "git pull origin $something:$cur

Of the nearly 20,000 commits in the Git source code history, this command shows the 6 that match those criteria.

### Using a GUI to Visualize History ###

If you like to use a more graphical tool to visualize your commit history, you may want to take a look at a Tcl/Tk program called gitk that is distributed with Git. Gitk is basically a visual `git log` tool, and it accepts nearly all the filtering options that `git log` does. If you type gitk on the command line in your project, you should see something like Figure 2-2.

Insert 18333fig0202.png
Figure 2-2. The gitk history visualizer.

You can see the commit history in the top half of the window along with a nice ancestry graph. The diff viewer in the bottom half of the window shows you the changes introduced at any commit you click.

## Undoing Things ##

At any stage, you may want to undo something. Here, we’ll review a few basic tools for undoing changes that you’ve made. Be careful, because you can’t always undo some of these undos. This is one of the few areas in Git where you may lose some work if you do it wrong.

### Changing Your Last Commit ###

One of the common undos takes place when you commit too early and possibly forget to add some files, or you mess up your commit message. If you want to try that commit again, you can run commit with the `--amend` option:

	$ git commit --amend

This command takes your staging area and uses it for the commit. If you’ve have made no changes since your last commit (for instance, you run this command immediately after your previous commit), then your snapshot will look exactly the same and all you’ll change is your commit message.

The same commit-message editor fires up, but it already contains the message of your previous commit. You can edit the message the same as always, but it overwrites your previous commit.

As an example, if you commit and then realize you forgot to stage the changes in a file you wanted to add to this commit, you can do something like this:

	$ git commit -m 'initial commit'
	$ git add forgotten_file
	$ git commit --amend

All three of these commands end up with a single commit — the second commit replaces the results of the first.

### Unstaging a Staged File ###

The next two sections demonstrate how to wrangle your staging area and working directory changes. The nice part is that the command you use to determine the state of those two areas also reminds you how to undo changes to them. For example, let’s say you’ve changed two files and want to commit them as two separate changes, but you accidentally type `git add *` and stage them both. How can you unstage one of the two? The `git status` command reminds you:

	$ git add .
	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#       modified:   README.txt
	#       modified:   benchmarks.rb
	#

Right below the “Changes to be committed” text, it says use `git reset HEAD <file>...` to unstage. So, let’s use that advice to unstage the benchmarks.rb file:

	$ git reset HEAD benchmarks.rb
	benchmarks.rb: locally modified
	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#       modified:   README.txt
	#
	# Changed but not updated:
	#   (use "git add <file>..." to update what will be committed)
	#   (use "git checkout -- <file>..." to discard changes in working directory)
	#
	#       modified:   benchmarks.rb
	#

The command is a bit strange, but it works. The benchmarks.rb file is modified but once again unstaged.

### Unmodifying a Modified File ###

What if you realize that you don’t want to keep your changes to the benchmarks.rb file? How can you easily unmodify it — revert it back to what it looked like when you last committed (or initially cloned, or however you got it into your working directory)? Luckily, `git status` tells you how to do that, too. In the last example output, the unstaged area looks like this:

	# Changed but not updated:
	#   (use "git add <file>..." to update what will be committed)
	#   (use "git checkout -- <file>..." to discard changes in working directory)
	#
	#       modified:   benchmarks.rb
	#

It tells you pretty explicitly how to discard the changes you’ve made (at least, the newer versions of Git, 1.6.1 and later, do this — if you have an older version, we highly recommend upgrading it to get some of these nicer usability features). Let’s do what it says:

	$ git checkout -- benchmarks.rb
	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#       modified:   README.txt
	#

You can see that the changes have been reverted. You should also realize that this is a dangerous command: any changes you made to that file are gone — you just copied another file over it. Don’t ever use this command unless you absolutely know that you don’t want the file. If you just need to get it out of the way, we’ll go over stashing and branching in the next chapter; these are generally better ways to go.

Remember, anything that is committed in Git can almost always be recovered. Even commits that were on branches that were deleted or commits that were overwritten with an `--amend` commit can be recovered (see Chapter 9 for data recovery). However, anything you lose that was never committed is likely never to be seen again.

## Working with Remotes ##

To be able to collaborate on any Git project, you need to know how to manage your remote repositories. Remote repositories are versions of your project that are hosted on the Internet or network somewhere. You can have several of them, each of which generally is either read-only or read/write for you. Collaborating with others involves managing these remote repositories and pushing and pulling data to and from them when you need to share work.
Managing remote repositories includes knowing how to add remote repositories, remove remotes that are no longer valid, manage various remote branches and define them as being tracked or not, and more. In this section, we’ll cover these remote-management skills.

### Showing Your Remotes ###

To see which remote servers you have configured, you can run the git remote command. It lists the shortnames of each remote handle you’ve specified. If you’ve cloned your repository, you should at least see origin — that is the default name Git gives to the server you cloned from:

	$ git clone git://github.com/schacon/ticgit.git
	Initialized empty Git repository in /private/tmp/ticgit/.git/
	remote: Counting objects: 595, done.
	remote: Compressing objects: 100% (269/269), done.
	remote: Total 595 (delta 255), reused 589 (delta 253)
	Receiving objects: 100% (595/595), 73.31 KiB | 1 KiB/s, done.
	Resolving deltas: 100% (255/255), done.
	$ cd ticgit
	$ git remote
	origin

You can also specify `-v`, which shows you the URL that Git has stored for the shortname to be expanded to:

	$ git remote -v
	origin	git://github.com/schacon/ticgit.git

If you have more than one remote, the command lists them all. For example, my Grit repository looks something like this.

	$ cd grit
	$ git remote -v
	bakkdoor  git://github.com/bakkdoor/grit.git
	cho45     git://github.com/cho45/grit.git
	defunkt   git://github.com/defunkt/grit.git
	koke      git://github.com/koke/grit.git
	origin    git@github.com:mojombo/grit.git

This means we can pull contributions from any of these users pretty easily. But notice that only the origin remote is an SSH URL, so it’s the only one I can push to (we’ll cover why this is in Chapter 4).

### Adding Remote Repositories ###

I’ve mentioned and given some demonstrations of adding remote repositories in previous sections, but here is how to do it explicitly. To add a new remote Git repository as a shortname you can reference easily, run `git remote add [shortname] [url]`:

	$ git remote
	origin
	$ git remote add pb git://github.com/paulboone/ticgit.git
	$ git remote -v
	origin	git://github.com/schacon/ticgit.git
	pb	git://github.com/paulboone/ticgit.git

Now you can use the string pb on the command line in lieu of the whole URL. For example, if you want to fetch all the information that Paul has but that you don’t yet have in your repository, you can run git fetch pb:

	$ git fetch pb
	remote: Counting objects: 58, done.
	remote: Compressing objects: 100% (41/41), done.
	remote: Total 44 (delta 24), reused 1 (delta 0)
	Unpacking objects: 100% (44/44), done.
	From git://github.com/paulboone/ticgit
	 * [new branch]      master     -> pb/master
	 * [new branch]      ticgit     -> pb/ticgit

Paul’s master branch is accessible locally as `pb/master` — you can merge it into one of your branches, or you can check out a local branch at that point if you want to inspect it.

### Fetching and Pulling from Your Remotes ###

As you just saw, to get data from your remote projects, you can run:

	$ git fetch [remote-name]

The command goes out to that remote project and pulls down all the data from that remote project that you don’t have yet. After you do this, you should have references to all the branches from that remote, which you can merge in or inspect at any time. (We’ll go over what branches are and how to use them in much more detail in Chapter 3.)

If you clone a repository, the command automatically adds that remote repository under the name origin. So, `git fetch origin` fetches any new work that has been pushed to that server since you cloned (or last fetched from) it. It’s important to note that the fetch command pulls the data to your local repository — it doesn’t automatically merge it with any of your work or modify what you’re currently working on. You have to merge it manually into your work when you’re ready.

If you have a branch set up to track a remote branch (see the next section and Chapter 3 for more information), you can use the `git pull` command to automatically fetch and then merge a remote branch into your current branch. This may be an easier or more comfortable workflow for you; and by default, the `git clone` command automatically sets up your local master branch to track the remote master branch on the server you cloned from (assuming the remote has a master branch). Running `git pull` generally fetches data from the server you originally cloned from and automatically tries to merge it into the code you’re currently working on.

### Pushing to Your Remotes ###

When you have your project at a point that you want to share, you have to push it upstream. The command for this is simple: `git push [remote-name] [branch-name]`. If you want to push your master branch to your `origin` server (again, cloning generally sets up both of those names for you automatically), then you can run this to push your work back up to the server:

	$ git push origin master

This command works only if you cloned from a server to which you have write access and if nobody has pushed in the meantime. If you and someone else clone at the same time and they push upstream and then you push upstream, your push will rightly be rejected. You’ll have to pull down their work first and incorporate it into yours before you’ll be allowed to push. See Chapter 3 for more detailed information on how to push to remote servers.

### Inspecting a Remote ###

If you want to see more information about a particular remote, you can use the `git remote show [remote-name]` command. If you run this command with a particular shortname, such as `origin`, you get something like this:

	$ git remote show origin
	* remote origin
	  URL: git://github.com/schacon/ticgit.git
	  Remote branch merged with 'git pull' while on branch master
	    master
	  Tracked remote branches
	    master
	    ticgit

It lists the URL for the remote repository as well as the tracking branch information. The command helpfully tells you that if you’re on the master branch and you run `git pull`, it will automatically merge in the master branch on the remote after it fetches all the remote references. It also lists all the remote references it has pulled down.

That is a simple example you’re likely to encounter. When you’re using Git more heavily, however, you may see much more information from `git remote show`:

	$ git remote show origin
	* remote origin
	  URL: git@github.com:defunkt/github.git
	  Remote branch merged with 'git pull' while on branch issues
	    issues
	  Remote branch merged with 'git pull' while on branch master
	    master
	  New remote branches (next fetch will store in remotes/origin)
	    caching
	  Stale tracking branches (use 'git remote prune')
	    libwalker
	    walker2
	  Tracked remote branches
	    acl
	    apiv2
	    dashboard2
	    issues
	    master
	    postgres
	  Local branch pushed with 'git push'
	    master:master

This command shows which branch is automatically pushed when you run `git push` on certain branches. It also shows you which remote branches on the server you don’t yet have, which remote branches you have that have been removed from the server, and multiple branches that are automatically merged when you run `git pull`.

### Removing and Renaming Remotes ###

If you want to rename a reference, in newer versions of Git you can run `git remote rename` to change a remote’s shortname. For instance, if you want to rename `pb` to `paul`, you can do so with `git remote rename`:

	$ git remote rename pb paul
	$ git remote
	origin
	paul

It’s worth mentioning that this changes your remote branch names, too. What used to be referenced at `pb/master` is now at `paul/master`.

If you want to remove a reference for some reason — you’ve moved the server or are no longer using a particular mirror, or perhaps a contributor isn’t contributing anymore — you can use `git remote rm`:

	$ git remote rm paul
	$ git remote
	origin

## Tagging ##

Like most VCSs, Git has the ability to tag specific points in history as being important. Generally, people use this functionality to mark release points (v1.0, and so on). In this section, you’ll learn how to list the available tags, how to create new tags, and what the different types of tags are.

### Listing Your Tags ###

Listing the available tags in Git is straightforward. Just type `git tag`:

	$ git tag
	v0.1
	v1.3

This command lists the tags in alphabetical order; the order in which they appear has no real importance.

You can also search for tags with a particular pattern. The Git source repo, for instance, contains more than 240 tags. If you’re only interested in looking at the 1.4.2 series, you can run this:

	$ git tag -l 'v1.4.2.*'
	v1.4.2.1
	v1.4.2.2
	v1.4.2.3
	v1.4.2.4

### Creating Tags ###

Git uses two main types of tags: lightweight and annotated. A lightweight tag is very much like a branch that doesn’t change — it’s just a pointer to a specific commit. Annotated tags, however, are stored as full objects in the Git database. They’re checksummed; contain the tagger name, e-mail, and date; have a tagging message; and can be signed and verified with GNU Privacy Guard (GPG). It’s generally recommended that you create annotated tags so you can have all this information; but if you want a temporary tag or for some reason don’t want to keep the other information, lightweight tags are available too.

### Annotated Tags ###

Creating an annotated tag in Git is simple. The easiest way is to specify `-a` when you run the `tag` command:

	$ git tag -a v1.4 -m 'my version 1.4'
	$ git tag
	v0.1
	v1.3
	v1.4

The `-m` specifies a tagging message, which is stored with the tag. If you don’t specify a message for an annotated tag, Git launches your editor so you can type it in.

You can see the tag data along with the commit that was tagged by using the `git show` command:

	$ git show v1.4
	tag v1.4
	Tagger: Scott Chacon <schacon@gee-mail.com>
	Date:   Mon Feb 9 14:45:11 2009 -0800

	my version 1.4
	commit 15027957951b64cf874c3557a0f3547bd83b3ff6
	Merge: 4a447f7... a6b4c97...
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Sun Feb 8 19:02:46 2009 -0800

	    Merge branch 'experiment'

That shows the tagger information, the date the commit was tagged, and the annotation message before showing the commit information.

### Signed Tags ###

You can also sign your tags with GPG, assuming you have a private key. All you have to do is use `-s` instead of `-a`:

	$ git tag -s v1.5 -m 'my signed 1.5 tag'
	You need a passphrase to unlock the secret key for
	user: "Scott Chacon <schacon@gee-mail.com>"
	1024-bit DSA key, ID F721C45A, created 2009-02-09

If you run `git show` on that tag, you can see your GPG signature attached to it:

	$ git show v1.5
	tag v1.5
	Tagger: Scott Chacon <schacon@gee-mail.com>
	Date:   Mon Feb 9 15:22:20 2009 -0800

	my signed 1.5 tag
	-----BEGIN PGP SIGNATURE-----
	Version: GnuPG v1.4.8 (Darwin)

	iEYEABECAAYFAkmQurIACgkQON3DxfchxFr5cACeIMN+ZxLKggJQf0QYiQBwgySN
	Ki0An2JeAVUCAiJ7Ox6ZEtK+NvZAj82/
	=WryJ
	-----END PGP SIGNATURE-----
	commit 15027957951b64cf874c3557a0f3547bd83b3ff6
	Merge: 4a447f7... a6b4c97...
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Sun Feb 8 19:02:46 2009 -0800

	    Merge branch 'experiment'

A bit later, you’ll learn how to verify signed tags.

### Lightweight Tags ###

Another way to tag commits is with a lightweight tag. This is basically the commit checksum stored in a file — no other information is kept. To create a lightweight tag, don’t supply the `-a`, `-s`, or `-m` option:

	$ git tag v1.4-lw
	$ git tag
	v0.1
	v1.3
	v1.4
	v1.4-lw
	v1.5

This time, if you run `git show` on the tag, you don’t see the extra tag information. The command just shows the commit:

	$ git show v1.4-lw
	commit 15027957951b64cf874c3557a0f3547bd83b3ff6
	Merge: 4a447f7... a6b4c97...
	Author: Scott Chacon <schacon@gee-mail.com>
	Date:   Sun Feb 8 19:02:46 2009 -0800

	    Merge branch 'experiment'

### Verifying Tags ###

To verify a signed tag, you use `git tag -v [tag-name]`. This command uses GPG to verify the signature. You need the signer’s public key in your keyring for this to work properly:

	$ git tag -v v1.4.2.1
	object 883653babd8ee7ea23e6a5c392bb739348b1eb61
	type commit
	tag v1.4.2.1
	tagger Junio C Hamano <junkio@cox.net> 1158138501 -0700

	GIT 1.4.2.1

	Minor fixes since 1.4.2, including git-mv and git-http with alternates.
	gpg: Signature made Wed Sep 13 02:08:25 2006 PDT using DSA key ID F3119B9A
	gpg: Good signature from "Junio C Hamano <junkio@cox.net>"
	gpg:                 aka "[jpeg image of size 1513]"
	Primary key fingerprint: 3565 2A26 2040 E066 C9A7  4A7D C0C6 D9A4 F311 9B9A

If you don’t have the signer’s public key, you get something like this instead:

	gpg: Signature made Wed Sep 13 02:08:25 2006 PDT using DSA key ID F3119B9A
	gpg: Can't check signature: public key not found
	error: could not verify the tag 'v1.4.2.1'

### Tagging Later ###

You can also tag commits after you’ve moved past them. Suppose your commit history looks like this:

	$ git log --pretty=oneline
	15027957951b64cf874c3557a0f3547bd83b3ff6 Merge branch 'experiment'
	a6b4c97498bd301d84096da251c98a07c7723e65 beginning write support
	0d52aaab4479697da7686c15f77a3d64d9165190 one more thing
	6d52a271eda8725415634dd79daabbc4d9b6008e Merge branch 'experiment'
	0b7434d86859cc7b8c3d5e1dddfed66ff742fcbc added a commit function
	4682c3261057305bdd616e23b64b0857d832627b added a todo file
	166ae0c4d3f420721acbb115cc33848dfcc2121a started write support
	9fceb02d0ae598e95dc970b74767f19372d61af8 updated rakefile
	964f16d36dfccde844893cac5b347e7b3d44abbc commit the todo
	8a5cbc430f1a9c3d00faaeffd07798508422908a updated readme

Now, suppose you forgot to tag the project at v1.2, which was at the "updated rakefile" commit. You can add it after the fact. To tag that commit, you specify the commit checksum (or part of it) at the end of the command:

	$ git tag -a v1.2 9fceb02

You can see that you’ve tagged the commit:

	$ git tag
	v0.1
	v1.2
	v1.3
	v1.4
	v1.4-lw
	v1.5

	$ git show v1.2
	tag v1.2
	Tagger: Scott Chacon <schacon@gee-mail.com>
	Date:   Mon Feb 9 15:32:16 2009 -0800

	version 1.2
	commit 9fceb02d0ae598e95dc970b74767f19372d61af8
	Author: Magnus Chacon <mchacon@gee-mail.com>
	Date:   Sun Apr 27 20:43:35 2008 -0700

	    updated rakefile
	...

### Sharing Tags ###

By default, the `git push` command doesn’t transfer tags to remote servers. You will have to explicitly push tags to a shared server after you have created them.  This process is just like sharing remote branches — you can run `git push origin [tagname]`.

	$ git push origin v1.5
	Counting objects: 50, done.
	Compressing objects: 100% (38/38), done.
	Writing objects: 100% (44/44), 4.56 KiB, done.
	Total 44 (delta 18), reused 8 (delta 1)
	To git@github.com:schacon/simplegit.git
	* [new tag]         v1.5 -> v1.5

If you have a lot of tags that you want to push up at once, you can also use the `--tags` option to the `git push` command.  This will transfer all of your tags to the remote server that are not already there.

	$ git push origin --tags
	Counting objects: 50, done.
	Compressing objects: 100% (38/38), done.
	Writing objects: 100% (44/44), 4.56 KiB, done.
	Total 44 (delta 18), reused 8 (delta 1)
	To git@github.com:schacon/simplegit.git
	 * [new tag]         v0.1 -> v0.1
	 * [new tag]         v1.2 -> v1.2
	 * [new tag]         v1.4 -> v1.4
	 * [new tag]         v1.4-lw -> v1.4-lw
	 * [new tag]         v1.5 -> v1.5

Now, when someone else clones or pulls from your repository, they will get all your tags as well.

## Tips and Tricks ##

Before we finish this chapter on basic Git, a few little tips and tricks may make your Git experience a bit simpler, easier, or more familiar. Many people use Git without using any of these tips, and we won’t refer to them or assume you’ve used them later in the book; but you should probably know how to do them.

### Auto-Completion ###

If you use the Bash shell, Git comes with a nice auto-completion script you can enable. Download the Git source code, and look in the `contrib/completion` directory; there should be a file called `git-completion.bash`. Copy this file to your home directory, and add this to your `.bashrc` file:

	source ~/.git-completion.bash

If you want to set up Git to automatically have Bash shell completion for all users, copy this script to the `/opt/local/etc/bash_completion.d` directory on Mac systems or to the `/etc/bash_completion.d/` directory on Linux systems. This is a directory of scripts that Bash will automatically load to provide shell completions.

If you’re using Windows with Git Bash, which is the default when installing Git on Windows with msysGit, auto-completion should be preconfigured.

Press the Tab key when you’re writing a Git command, and it should return a set of suggestions for you to pick from:

	$ git co<tab><tab>
	commit config

In this case, typing git co and then pressing the Tab key twice suggests commit and config. Adding `m<tab>` completes `git commit` automatically.

This also works with options, which is probably more useful. For instance, if you’re running a `git log` command and can’t remember one of the options, you can start typing it and press Tab to see what matches:

	$ git log --s<tab>
	--shortstat  --since=  --src-prefix=  --stat   --summary

That’s a pretty nice trick and may save you some time and documentation reading.

### Git Aliases ###

Git doesn’t infer your command if you type it in partially. If you don’t want to type the entire text of each of the Git commands, you can easily set up an alias for each command using `git config`. Here are a couple of examples you may want to set up:

	$ git config --global alias.co checkout
	$ git config --global alias.br branch
	$ git config --global alias.ci commit
	$ git config --global alias.st status

This means that, for example, instead of typing `git commit`, you just need to type `git ci`. As you go on using Git, you’ll probably use other commands frequently as well; in this case, don’t hesitate to create new aliases.

This technique can also be very useful in creating commands that you think should exist. For example, to correct the usability problem you encountered with unstaging a file, you can add your own unstage alias to Git:

	$ git config --global alias.unstage 'reset HEAD --'

This makes the following two commands equivalent:

	$ git unstage fileA
	$ git reset HEAD fileA

This seems a bit clearer. It’s also common to add a `last` command, like this:

	$ git config --global alias.last 'log -1 HEAD'

This way, you can see the last commit easily:

	$ git last
	commit 66938dae3329c7aebe598c2246a8e6af90d04646
	Author: Josh Goebel <dreamer3@example.com>
	Date:   Tue Aug 26 19:48:51 2008 +0800

	    test for current head

	    Signed-off-by: Scott Chacon <schacon@example.com>

As you can tell, Git simply replaces the new command with whatever you alias it for. However, maybe you want to run an external command, rather than a Git subcommand. In that case, you start the command with a `!` character. This is useful if you write your own tools that work with a Git repository. We can demonstrate by aliasing `git visual` to run `gitk`:

	$ git config --global alias.visual "!gitk"

## Summary ##

At this point, you can do all the basic local Git operations — creating or cloning a repository, making changes, staging and committing those changes, and viewing the history of all the changes the repository has been through. Next, we’ll cover Git’s killer feature: its branching model.
