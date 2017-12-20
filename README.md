##Serwer Socket IO:
1. Po zalogowaniu do Uniclouda klikamy u góry zielony przycisk "Utwórz środowisko".
2. W okienku które się pojawiło wybierami zakładkę "Nodej.js", w polu "Nazwa środowiska" wpisujemy naszą unikalną subdomenę oraz zatwierdzamy przyciskiem "Utwórz".
3. Czekamy do 15 minut aż środowisko zostanie utworzone. Status zmieni się wtedy na uruchomione, a po prawej stronie wyskoczy komunikat że środowisko jest utworzone.
4. Najeżdzamy myszką na "Nodejs 0.x-6.x" i wybieramy ikoknę "Dodaj Projekt".
5. W okienku które wyskoczyło wybieramy zakładkę "Git" i w polu URL wklejamy adres:
https://github.com/Policjant/socketsrv
Następnie klikami przycisk "Dodaj".
6. I gotowe - serwer socket.io zainstalowany. Przechodząc pod subodomenę *.unicloud.pl jaką wybraliśmy dla naszego projektu możemy generować tokeny dla prezentacji.


##Podpinanie Multiplexingu do prezentacji:
1. Otwieramy w edytorze plik .html z naszą prezentacją lub z przykładową pobraną ze strony autora: https://github.com/hakimel/reveal.js/archive/master.zip
2. Domyślną inicjalizację skryptu Reveal.js znajdującą się przed tagiem </body>:
```javascript
		<script>
			// More info about config & dependencies:
			// - https://github.com/hakimel/reveal.js#configuration
			// - https://github.com/hakimel/reveal.js#dependencies
			Reveal.initialize({
				dependencies: [
					{ src: 'plugin/markdown/marked.js' },
					{ src: 'plugin/markdown/markdown.js' },
					{ src: 'plugin/notes/notes.js', async: true },
					{ src: 'plugin/highlight/highlight.js', async: true, callback: function() { hljs.initHighlightingOnLoad(); } }
				]
			});
		</script>
```

zastępujemy kodem:
```javascript
		<script src="https://cdnjs.cloudflare.com/ajax/libs/socket.io/2.0.4/socket.io.slim.js" integrity="sha256-jniDwC1PC9OmGoyPxA9VpGvgwDYyxsMqu5Q4OrF5wNY=" crossorigin="anonymous"></script>

		<script>
			
			Reveal.initialize({
					multiplex: {
						url: 'ADRES_SERVERA',
						id: 'SOCKET_ID',
						secret: Reveal.getQueryHash().master || null
					},
				dependencies: [
			
						{ src: Reveal.getQueryHash().master ?
					  'plugin/multiplex/master.js' :
					  'plugin/multiplex/client.js', async: true }
				]
				})
		</script>
```

3. Kolejnym krokiem jest przejście pod adres naszego serwera socket.io (jeśli instalowaliśmy go na unicloud to adresem jest wybrana przez nas subdomena w domenie *.unicloud.pl). 
Pod adresem serwer znajdziemy generator tokenów. Klikamy na odnośnik "Generate Token" i kopiujemy do notatnika cały wygenerowany ciąg, który powinien wygląda podobnie do tego:
> {"secret":"640c71d732845b64","socketId":"67fd85f288acf43f"}

4. Teraz bardzo ważny krok! We wklejonym wcześniej skrypcie inicjalizującym Reveals.js nadpisujemy tekst "ADRES_SERVERA" naszym serwerm socket.io (ten na unicloud!),
natomiast tekst "SOCKET_ID" nadpisujemy wygenerowanym tokenem socketId czyli w moim przypadku "67fd85f288acf43f".

5. Teraz wystarczy wrzucić prezentację na dowolny serwer (nie musi posiadać node.js) i GOTOWE!

7. DO administracji serwera wystarczy dopisać do adresu ?master=SECRET gdzie jako SECRET podstawiamy wygenerowany secret z tokena czyli w moim przypadku "640c71d732845b64".
