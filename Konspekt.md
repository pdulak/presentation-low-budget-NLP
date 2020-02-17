### Kim jestem

Nazywam się Paweł Dulak i jestem programistą języka ColdFusion w firmie Binary Minds. ColdFusion jest głównie wykorzystywany w tworzeniu stron internetowych i tym właśnie zajmujemy się w naszym software house. Pracujemy dla klientów w USA.

### Nasza praca

Czasem porównuję nasza pracę do serwisu samochodowego - w serwisie przyjeżdża klient i mówi - proszę zmienić klocki hamulcowe, coś mi w zawieszeniu stuka... U nas najczęściej słyszymy - potrzebuję nowego raportu, zaczynam sprzedawać nowy produkt i potrzebuję innego wyglądu strony produktowej itp. Czasem jest to tak proste jak zmiana koloru na innego wyglądu strony produktowej itp. Czasem jest to tak proste jak zmiana koloru na jednym z przycisków a czasem piszemy aplikację od A do Z pod konkretne potrzeby klienta. Jednak tych drobiazgów jest więcej.

Nie jesteśmy zatem fabryką samochodową która potrzebuje dwóch lat i sztabu inżynierów na wyprodukowanie nowego samochodu. Nie mamy (póki co) własnego produktu. Nasi klienci liczą na to że rozwiążemy ich biznesowy problem - w miarę szybko i w miarę tanio, a przede wszystkim dobrze. Od tego w końcu zależy ich biznes.

### Moda na ML

Machine Learning staje się obecnie tak popularny, że nawet nasi najbardziej nietechniczni klienci zaczynają go zauważać. Coraz częściej spotykamy się z pytaniami "a czy robicie też sztuczną inteligencję?" Czasem spędzamy sporo czasu wymieniając maile albo rozmawiając przez telefon. Okazuje się więc że coraz bardziej potrzebujemy ludzi którzy będą potrafili przekazać klientowi czym ML i AI jest, jakie problemy może rozwiazać a kiedy nie jest potrzebna. Nawet na bardzo podstawowym poziomie. Rola ​ edukacji​ jest tutaj bardzo ważna.

Edukujemy o tym że warto zbierać dane, że warto przyglądać się naszym problemom biznesowym, starać się zauważać gdzie ​ nasi klienci​ oraz ​ klienci naszych klientów​ mają problemy z którymi możemy sobie poradzić.

### Maile od Constance

Opowiem Wam o jednym przypadku który mieliśmy. Zaczęło się dość niewinnie od maila od klientki:

_I am facing many competitors ​ who are able to provide ​ sentiment analysis ​ and text categorization ​ in their applications. Does anyone on your team have experience designing a text mining function?_

Odpowiedź:

_This is easily available through AWS Comprehend: https://aws.amazon.com/comprehend/ I performed an analysis of about 2300 recent comments on MV Surveys, you can review the results on the website (see attached screenshot)_

I jej odpowiedź:

_The sentiment analysis is very accurate and I love it._

Możemy zatem użyć AWS Comprehend do sentiment analysis.

### Problem z klasyfikacją

Czy można robić też klasyfikację? Ano można by, tylko pytanie na czym wyszkolić model? Jesli chodzi o sentiment analysis - sprawa jest prosta. Wiele osób to robi i są standardowe biblioteki które zawierają nauczony model, nie trzeba nic więcej zrobić. Jeśli jednak chodzi o klasyfikcję i to jeszcze w dziedzinach specyficznych dla konkretnego klienta, to potrzeba taki model NLP nauczyć. Albo prościej - wziąć wstępnie nauczony model i przystosować go do naszych potrzeb.

Gdzie leży problem? W braku danych. Klient przysłał nam po 2-10 przykładów komentarzy które wpadają w daną kategorię. No i klops, na tym zbiorze danych nie nauczymy sensownego modelu.

### Nasza propozycja

Zaproponowaliśmy i wdrożyliśmy sentiment analysis na gruncie AWS Comprehend. Przygotowaliśmy też interfejs w którym klient mógł wybierać kategorię do komentarza. Mieliśmy zatem rozwiazany jeden problem a do drugiego zbieraliśmy dane. Jakiś czas później dostaliśmy kolejnego maila:

_I am exploring deploying Google's open source BERT over our data and then having your team write reports to present and analyze the data on the client side. Here's my question: Does Binary Minds have any experience with machine learning, specifically BERT? I know that Google has made it available publicly._

Muszę przyznać że po pierwszym przeczytaniu moco się zestresowałem. Nie mieliśmy głębszego doświadczenia z NLP nie wspominająć o BERT. Spędziłem trochę czasu czytając w czym rzecz i orientując się jak możemy wykorzystać ten wstępnie wytrenowany model. Okazało się że maszyna na której jest sens robić fine-tuning tego modelu to minimum 32GB ramu i GPU. Ceny takich instancji zaczynają się od około dolara za godzinę, co daje ponad 700 dolarów​ na miesiąc. Nawet jeśli po wyszkoleniu BERTa przeszli byśmy na mniejszą instancję, nadal było to ponad ​ 300 dolarów​ miesięcznie. Dla małej firmy to spory wydatek a dodatkowo dochodzi utrzymanie w sensie aktulaizacji, zapewnienia bezpieczeństwa itp.

Po drugiej stronie mieliśmy dalej AWS i tym razem Comprehend Custom Classifier. Przez czas który minął od uruchomienia analizy wydźwięku mieliśmy już uzbierane ponad 100.000 sklasyfikowanych komentarzy. Jak to bywa w życiu, część kategorii miała bardzo mało komentarzy (poniżej 50) a w części mogliśmy wybierać z tysięcy przykładów.

Szkolenie Custom Classifier zabrało około 20 minut. Testowanie na pozostałych danych kolejne 20 minut. Za całość zapłaciliśmy $15.

W ramach testu klient udostępnił nam jeden miesiąc danych z konkretnego oddziału i poprosił o ich sklasyfikowanie. Przygotowaliśmy dane i zaprezentowaliśmy klientowi:

_In some cases, I agreed more with the Amazon classifier than with the manual classifier. In others, it was the other way around, but when looking at classifiers that are 25% or greater probability, there seems to be a fairly good match._

### Koszty

Okazało się zatem że sprzedaliśmy klientowi dwa nie swoje modele - AWS Comprehend Sentiment Analisis i Custom Classifier. Kosztowało nas to parnaście godzin pracy. Dwa, może trzy dni pracy. Wyniki są satysfakcjonujące dla biznesu a w przyszłości możemy je ulepszać biorąc pod uwagę że Customer Service poprawia klasyfikację błędnie sklasyfikowanych komentarzy.

A koszty?

\[slajd z danymi z AWS\]

Plusy:

- brak narzutu na utrzymanie maszyny z modelem

- skaluje się w miarę potrzeb

- niskie koszty

Minusy:

- nie mamy kontroli nad modelem

- mogą go wyłączyć w każdej chwili

- nie brzmi tak dobrze w CV jak coś co zrobiło się samemu
