# LM

Zostało wykorzystane narzędzie SRILM: http://www.speech.sri.com/projects/srilm/

Tworzymy unigramy:
```
ngram-count -text task3_train_segmented.txt -write total_unigrams -order 1 -sort
```
W korpusie jest 4165635 różnych tokenów

Na podstawie unigramów tworzymy słownik:
```
awk '{ if ($2 >= 3 ) print $1 }' total_unigrams > vocabulary
```
Słownik zawiera 1378029 tokenów.

Zliczamy ngramy 5 rzędu:
```
ngram-count -text task3_train_segmented.txt -write o5.ngrams -order 5 -sort -vocab vocabulary
```
5-gramy zajmują 24GB.

Tworzymy model:
```
ngram-count -read o5.ngrams -lm language_model -name language_model -order 5 -sort -unk -vocab vocabulary -kndiscount -interpolate
```
Model zajmuje 4,8GB.

Testujemy model:
```
ngram -lm language_model -ppl task3_test_segmented.txt -order 5 -unk
```
```
2556845 sentences, 50218535 words, 0 OOVs
0 zeroprobs, logprob= -1.143355e+08 ppl= 146.7082 ppl1= 189.129
```

Perplexity: 146.7082

Procent nieznanych słów w korpusie testowym: 0.868323449305339%

Model znajduje się pod adresem: http://wierzba.wzks.uj.edu.pl/~kwrobel/LM/lm_o5.gz
