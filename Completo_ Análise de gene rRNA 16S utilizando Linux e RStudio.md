# Completo: Análise de gene rRNA 16S utilizando Linux e RStudio
Escrito por: *Isadora Soares de Lima*

### **Qiime 2**

**1) Instalando Miniconda** 

Entre no [site](https://docs.conda.io/en/latest/miniconda.html) do Conda, um sistema de gerenciamento de pacotes e de ambientes virtuais, instalando, executando e atualizando rapidamente pacotes e suas dependências. Além disso, o sistema Conda cria, salva, carrega e alterna facilmente entre ambientes presentes no seu computador.

No tópico **'Linux installers'**, clique no link presente na coluna *'Name'* do Miniconda correspondente à versão do seu Python, e à quantidade de bits do seu Linux.  

![](https://i.imgur.com/Ya7492E.png)

O Miniconda é um instalador para o Conda, possuindo alguns dos pacotes dos quais o mesmo necessita, como *pip*, *zlib* e outros.

A instalação será feita no terminal do seu computador. Para abrí-lo, aperte as teclas `Ctrl + Alt + T`. Pronto, é nesta tela que todas as instalações e grande parte da sua análise será feita. 

Agora execute os seguintes comandos:
```coffeescript=
cd Downloads/
bash ARQUIVO_MINICONDA_BAIXADO
# Aperte ENTER até que o sistema peça a sua autorização para terminar a instalação. Aparecerá assim no seu terminal:
[s] or [n]
# Aperte a tecla `S` do seu teclado para autorizar
```
Feche o seu terminal e, em seguida, abra um novo terminal. Feito isso, você pode instalar o Qiime 2.

Obs.: Para aprender outros comandos básicos do Linux, você pode entrar neste [site](https://www.devmedia.com.br/comandos-importantes-linux/23893).

**2) Instalando Qiime 2**

Após instalar o Miniconda, verifique se você está executando a versão mais recente do Conda:

```coffeescript=
conda update conda
conda install wget
```

Entre neste [site](https://docs.qiime2.org/2019.10/install/native/#install-miniconda) e, no tópico **'Install QIIME 2 within a conda environment'**, selecione a opção Linux (64-bit) e copie e cole no seu terminal apenas a primeira linha dos comandos apresentados, como indicado na imagem a seguir: 

![](https://i.imgur.com/8C7iIAK.png)

Agora, copie a segunda linha dos comandos, seguindo a indicação da imagem:

![](https://i.imgur.com/35Na7yT.png)

Para ativar o Qiime 2, basta encontrar o arquivo *.yml* que foi baixado (possivelmente na sua pasta *Downloads*) e, no terminal, usar o comando `conda activate` e o nome deste arquivo, digitando somente até a versão do Qiime 2. Exemplo:

```coffeescript=
conda activate qiime2-2019.7
```
Para testar se o Qiime 2 realmente foi instalado, use o comando:

```coffeescript=
qiime --help
```

e veja se o output é uma descrição do Qiime 2, com opções e comandos:

![](https://i.imgur.com/O1SW24X.png)

Para desativar o ambiente virtual,  basta usar o comando:

```coffeescript=
conda deactivate 
```

### **Bioinfo**

Depois de desativar o Qiime 2, agora você vai instalar o Bioinfo, outro ambiente virtual. Digite no seu terminal:

```coffeescript=
conda create -n bioinfo python
```
Pronto, está instalado. Para ativar o Bioinfo, use o comando:

```coffeescript=
conda activate bioinfo
```
Para desativá-lo, use o comando:

```coffeescript=
conda deactivate
```

---

## **Iniciando as Análises**

Antes de começar as análises, é importante que crie uma pasta principal onde serão criadas todas as pastas que você usará. Esta pode ser chamada, por exemplo, de **Análises_Teste**. Dentro desta pasta, organize os dados brutos que serão avaliados em outra pasta, para tornar o processo mais rápido e fácil. Para o tutorial, a nomearei de **Dados_Brutos**.

Outra informação que vale ressaltar antes de iniciar este tutorial é: quando você se deparar com a informação CAMINHO_PARA_PASTA em algum código, estou me referindo ao endereço para cada pasta do seu computador. Se, por exemplo, eu precisar do caminho para pasta **Dados_Brutos** do meu computador, esse seria o caminho: 

**/home/isadora/Documentos/Análises_Teste/Dados_Brutos**

Você pode encontrar este caminho entrando na pasta que você quer saber o endereço e apertando as teclas `Ctrl+L`. Aperte as teclas `Ctrl+C` para copiar esse caminho e cole no local que precisar com `Ctrl+V`.

Entendendo isso, vamos começar!


---
### Passo 1: Análise de qualidade no FastQC

O FastQC é uma ferramenta de controle de qualidade para dados de sequências de alto rendimento, que tem como objetivo fornecer um relatório de CQ que pode detectar problemas originados no sequenciador ou no material inicial da biblioteca. Por isso, é atualmente muito utilizado para realizar análises de sequências nucleotídicas em estudos biológicos.

Você pode saber mais informações sobre esta ferramenta no [manual](https://dnacore.missouri.edu/PDF/FastQC_Manual.pdf) do FastQC.

#### **1) Análise individual das amostras**
Comece instalando o FastQC no seu computador. Tudo o que vamos fazer neste momento será realizado no Bioinfo. 
```coffeescript=
# Primeiro, ative o Bioinfo
conda activate bioinfo
# Agora instale o pacote
conda install -c bioconda/label/cf201901 fastqc
```
Para fazer as análises individuais, você pode seguir dois caminhos: (1) pode analisar amostra por amostra, usando um código para cada uma delas; ou (2) analisar todas as amostras de forma individual, mas usando um único código para todas elas juntas.

(1) 
```coffeescript=
# Entre na pasta principal
cd Análises_Teste
# Crie uma pasta para colocar os arquivos de saída
mkdir 01.fastqc_out
# Entre na pasta com dados brutos
cd Dados_Brutos
# Use esse código no terminal
fastqc ARQUIVO.fastq -o 01.fastqc_out
# Depois de analisar todas as amostras que você possui, saia da pasta Dados_Brutos
cd ..
```

(2)
```coffeescript=
# Entre na pasta principal
cd Análises_Teste
# Crie uma pasta para colocar os arquivos de saída
mkdir 01.fastqc_out
# Entre na pasta com dados brutos
cd Dados_Brutos
# Use esse código no terminal
fastqc *.fastq -o 01.fastqc_out
# Agora saia das pasta Dados_Brutos
cd ..
```
No terminal, para ambos os códigos usados, você vai obter uma leitura como esta abaixo para cada amostra:

![](https://i.imgur.com/fTc7DZ5.png)

Serão obtidos dois arquivos de saída: um arquivo .html e outro .zip. Abrindo o arquivo .html em seu navegador web de preferência, você terá acesso aos dados básicos da amostra e diversos gráficos de análise por base das sequências. Dentre eles, encontrará o gráfico de qualidade, que utiliza o índice de qualidade Phred, que indica a probabilidade de que uma determinada base tenha sido sequenciada incorretamente.

A pontuação mínima de qualidade utilizada como referência nos sistemas de sequenciamento Sanger é de 20 (Q20). Isso é equivalente à probabilidade de uma chamada de base incorreta 1 em 100 vezes, ou seja, a probabilidade de uma chamada de base correta é de 99%.

Já nos sequenciamentos de Nova Geração, a referência é a pontuação Q30, onde a acurácia na chamada de bases é de 99,9%.

Pontuações Q abaixo dos valores de referência, em ambos os métodos de sequenciamento, podem resultar em conclusões imprecisas.

Veja um exemplo deste tipo de gráfico:

![](https://i.imgur.com/KK2sv8o.png)

No eixo horizontal são dispostas as posições dos pares de bases, enquanto que no eixo vertical, as pontuações do índice de qualidade Phred.

#### **2) Análise das amostras combinadas**

Agora, você vai construir um gráfico de qualidade de todas as amostras unidas. Ou seja, na etapa anterior, foi feito um gráfico de qualidade para cada amostra de forma individual. Neste momento, será feito um único gráfico que mostrará a qualidade geral de todas as amostras juntas.

```coffeescript=
# Crie uma pasta para colocar os arquivos de saída
mkdir 02.fastqc_out_combined
# Entre na pasta Dados_Brutos
cd Dados_Brutos
# Use esse código no terminal
cat *fastq | fastqc stdin -o 02.fastqc_out_combined
# Saia da pasta
cd ..
```
Caso a qualidade observada nos gráficos das amostras esteja abaixo das referências apresentadas e este resultado não represente um prejuízo no seu trabalho, você pode pular o próximo passo. Caso contrário, você pode realizar uma análise mais detalhada das amostras seguindo o tutorial do **Passo 2**, onde as sequências serão verificadas a cada par de base e, se houver a necessidade, os fragmentos com baixa qualidade serão retirados.

---

## **Passo 2: Aparando as sequências com baixa qualidade**

Para melhorar a qualidade das amostras, você vai usar a ferramenta Trimmomatic. O Trimmomatic é uma ferramenta que pode ser usada para aparar e cortar dados de sequências FASTQ, bem como para remover adaptadores. 

Você pode saber mais informações sobre esta ferramenta no [manual](http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/TrimmomaticManual_V0.32.pdf) do Trimmomatic ou neste [site](http://www.usadellab.org/cms/?page=trimmomatic).

Para instalar o Trimmomatic, entre neste [site](https://anaconda.org/bioconda/trimmomatic) e use um dos dois comandos apresentados no site:

```coffeescript=
conda install -c bioconda trimmomatic
conda install -c bioconda/label/cf201901 trimmomatic
```
Agora entre neste [site](http://www.usadellab.org/cms/?page=trimmomatic) e, no tópico "Downloading Trimmomatic", faça o download do diretório do Trimmomatic contendo um arquivo *.jar*.
![](https://i.imgur.com/Juec0Zx.png)
Verifique a versão do seu Trimmomatic (você pode fazer isso digitando `conda list` no seu terminal) e clique na opção **binary** referente à versão da sua ferramenta. Será iniciado o download deste diretório. Você vai obter uma pasta *.zip*, que precisará ser movida para a pasta principal Análises_Teste.

```coffeescript=
# Saia da pasta principal
cd ..
# Agora você precisa entrar na pasta Downloads do seu computador
# Em seguida, use o seguinte comando, inserindo a versão do Trimmomatic correspondente (tire o aspas do código!):
mv Trimmomatic-(digite a versão aqui).zip CAMINHO_PARA_PASTA_PRINCIPAL
# Exemplo:
mv Trimmomatic-0.39.zip /home/isadora/Documentos/Análises_Teste
```
Depois de fazer isso, você precisa voltar para a pasta principal pelo seu terminal. Estando lá, você pode usar `ls` para se certificar que o arquivo *.zip* realmente foi movido. Em caso negativo, você pode mover manualmente a pasta de lugar. Em caso positivo, agora você vai extrair um diretório deste *.zip*, usando o código:
```coffeescript=
unzip Trimmomatic-(digite a versão aqui).zip 
```
Será extraída uma pasta com o mesmo nome do arquivo *.zip*. Dentro desta pasta está o arquivo *.jar* que será necessário para que o código seja executado corretamente. **Guarde o caminho que leva até este arquivo *.jar***. 
Agora você pode realizar a análise que precisa, mas antes é importante explicar algumas informações que serão colocadas no código do Trimmomatic.

Os parâmetros do Trimmomatic são:
* **PE:** indica paired end
* **LEADING:** remove bases com baixa qualidade no início da sequência (qualidade abaixo de um valor de sua escolha. No caso deste tutorial, foi estipulada qualidade mínima de 3)
* **TRAILING:** remove bases com baixa qualidade no final da sequência (qualidade abaixo de um valor de sua escolha. No caso deste tutorial, foi estipulada qualidade mínima de 3)
* **SLIDINGWINDOW:** utiliza uma janela deslizando com um valor N de bases (4 bases no exemplo deste tutorial) e corta quando a qualidade média é menor que um valor N (valor 15 neste tutorial)
* **MINLEN:** remove sequências menores que um número N de bases (36 bases neste tutorial)

Esses parâmetros podem ser alterados de acordo com os critérios do pesquisador.  

```coffeescript=
# Crie uma pasta 
mkdir 03.trimmomatic_output

# Para sequências unidas, use o código Single End do Trimmomatic, onde você precisa inserir o arquivo bruto e um nome para o arquivo de saída.
java -jar CAMINHO_PARA_ARQUIVO_JAR SE -phred33 Dados_Brutos/AMOSTRA_UNIDA.fastq 03.trimmomatic_output/NOME_ARQUIVO_SAÍDA.fastq.gz ILLUMINACLIP:TruSeq3-SE:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36
# Exemplo:
java -jar /home/isadora/Documentos/Análises_Teste/Trimmomatic-0.39/trimmomatic-0.39.jar SE -phred33 Dados_Brutos/ERS1294201.fastq 03.trimmomatic_output/ERS1294201_output.fastq.gz ILLUMINACLIP:TruSeq3-SE:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36

#Para sequências separadas em forward e reverse, use esse comando Paired End:
java -jar CAMINHO_PARA_ARQUIVO_JAR PE Dados_Brutos/ARQUIVO_FORWARD.fq.gz Dados_Brutos/ARQUIVO_REVERSE.fq.gz 03.trimmomatic_output/output_forward_paired.fq.gz 03.trimmomatic_output/output_forward_unpaired.fq.gz 03.trimmomatic_output/output_reverse_paired.fq.gz 03.trimmomatic_output/output_reverse_unpaired.fq.gz ILLUMINACLIP:TruSeq3-PE.fa:2:30:10:2:keepBothReads LEADING:3 TRAILING:3 MINLEN:36
```
No caso do comando *Singled End*, por exemplo, você pode ver no seu terminal algo parecido com o que se observa na imagem:

![](https://i.imgur.com/tOcInMP.png)

Para *Paired End*:

![](https://i.imgur.com/Bookfdu.png)

Observe que, ao final do ajuste de cada amostra, é informado o número de reads inserido, quantos reads foram mantidos e a sua porcentagem correspondente, e o número de reads eliminados e sua porcentagem correspondente. Isso pode te ajudar a decidir se foram perdidos muitos reads da sua amostra e se isso pode prejudicar os resultados e conclusões que serão inferidas após toda a análise. 

Para verificar se a qualidade das sequências realmente melhorou, use o FastQC mais uma vez.   

---

## Passo 3: Análise de qualidade das sequências aparadas

Dos arquivos de saída do Trimmomatic, use somente os *.fastq* das pastas **paired.fq.gz**. 

Você pode, mais uma vez, escolher qual código do FastQC vai usar, como mostrado no **Passo 1**. Não se esqueça de criar uma pasta para colocar os arquivos de saída!


```coffeescript=
# Crie uma pasta
mkdir 04.fastqc_trimmomatic
# Use um dos códigos que foram mostrados no Passo 1:
fastqc 03.trimmomatic_output/ARQUIVO_paired.fq.gz -o 04.fastqc_trimmomatic
# ou
fastqc 03.trimmomatic_output/*_paired.fq.gz -o 04.fastqc_trimmomatic
```

---

## Passo 4: Retirando os adaptadores (primers)
Estando as amostras devidamente ajustadas com relação à sua qualidade, você já pode retirar os primers presentes em cada sequência. Para isso, você precisa ter em mãos as sequências destes primers, tanto no sentido forward quanto reverse.

Dois exemplos de primers seriam o **515F** e **806R** que delimitam a região *V4* do gene 16S rDNA bacteriano: 

**515F**    GTGCCAGCMGCCGCGGTAA
**806R**	GGACTACHVGGGTWTCTAAT

Para retirar os adaptadores, você vai usar a ferramenta Cutadapt, que localiza e remove sequências de adaptadores, primers, caudas poli-A e outros tipos de sequência indesejada de suas sequências. Você pode acessar outras informações no [site](https://cutadapt.readthedocs.io/en/stable/) oficial do Cutadapt.

Tendo a lista de primers, use estes comandos abaixo: 
```coffeescript=
# Ainda no Bioinfo, instale o Cutadapt
conda install -c bioconda cutadapt
# Se ocorrer algum erro, use:
sudo apt install cutadapt
#Crie uma pasta
mkdir 05.cutadapt
# Agora você vai retirar os primers
cutadapt -g PRIMER_FORWARD -G PRIMER_REVERSE -o NOME_OUTPUT_FORWARD.fastq.gz -p NOME_OUTPUT_REVERSE.fastq.gz 03.trimmomatic_output/ARQUIVO_COM_PRIMER 03.trimmomatic_output/ARQUIVO_COM_PRIMER > NOME_DA_AMOSTRA.txt
# Agora você precisa mover esses arquivos para a pasta 07.cutadapt
mv OUTPUT_FORWARD.fastq.gz 05.cutadapt
mv OUTPUT_REVERSE.fastq.gz 05.cutadapt
mv AMOSTRA.txt 05.cutadapt
```
Ao final da análise, observe que será gerado um arquivo *.txt* de saída, que apresentará informações como: o comando utilizado no terminal, o tempo de análise, um sumário com informações gerais e, por último, dados sobre cada adaptador de forma individual. 

Exemplo:

![](https://i.imgur.com/cRjnf5r.png)
![](https://i.imgur.com/LwX62s4.png)

Este arquivo é importante para que você possa verificar a quantidade de reads forward e reverse que foram processados e de quantos reads foram retirados os primers. 

---

## Passo 5: Construindo um objeto Qiime

A partir de agora, para que as amostras que você possui possam ser analisadas pelo Qiime 2, você precisa construir um **arquivo de manifesto** com todas estas amostras. Ou seja, esse arquivo vai guardar metadados das amostras para processamento posterior através de ferramentas variadas.

**1) Montando o arquivo de manifesto**

```coffeescript=
# Desative o Bioinfo e ative o Qiime 2
# Permaneça na pasta principal
# Agora você vai construir um arquivo .txt que será lido pelo Qiime 2
nano ManifestFile
# Você será encaminhado para uma tela vazia, onde deverá ser feita uma tabela com os dados das amostras
# Cada coluna deve ser separada da outra sempre usando a tecla TAB
```
**IMPORTANTE:** O ManifestFile pode ser feito de duas formas, dependendo da memória RAM do seu computador. 

++-Muita memória RAM:++ faça um único arquivo de manifesto com todas as amostras;
++-Pouca memória RAM:++ faça um arquivo de manifesto para cada amostra, separadamente.

Neste tutorial, vou exemplificar um ManifestFile com todas as amostras juntas. Ou seja, para computadores com muita memória RAM. Mas caso você prefira fazer do outro modo, cada arquivo de manifesto terá duas linhas: a de títulos, e a linha com o caminho do forward e reverse de uma amostra.
```coffeescript=
# Sequências Paired-End
# Construa 3 colunas, uma com os IDs das amostras, outros para as amostras Forward e outro para as amostras Reverse
# Exemplo Paired_End Sequences:
sample-id    forward-absolute-filepath    reverse-absolute-filepath
amostra1    $PWD/Sequências_Trimmed/S224_L001_R1_001_cutadapt.fastq.gz    $PWD/Sequências_Trimmed/S224_L001_R2_001_cutadapt.fastq.gz
amostra2    $PWD/Sequências_Trimmed/S225_L001_R1_001_cutadapt.fastq.gz    $PWD/Sequências_Trimmed/S225_L001_R2_001_cutadapt.fastq.gz
amostra3    $PWD/Sequências_Trimmed/S226_L001_R1_001_cutadapt.fastq.gz    $PWD/Sequências_Trimmed/S226_L001_R2_001_cutadapt.fastq.gz
amostra4    $PWD/Sequências_Trimmed/S227_L001_R1_001_cutadapt.fastq.gz    $PWD/Sequências_Trimmed/S227_L001_R2_001_cutadapt.fastq.gz
amostra5    $PWD/Sequências_Trimmed/S228_L001_R1_001_cutadapt.fastq.gz    $PWD/Sequências_Trimmed/S228_L001_R2_001_cutadapt.fastq.gz
# Para sair, aperte Ctrl+X, depois S, coloque o nome do arquivo e ENTER

# Sequências Single-End
# Construa 2 colunas, uma com os IDs das amostras, e a outra para as amostras Single-End
# Exemplo Single-End Sequences
sample-id    absolute-filepath	
ERS255958	$PWD/Sequências_Brutas/ERS255958.fastq.gz   
ERS255959	$PWD/Sequências_Brutas/ERS255959.fastq.gz   
ERS255960	$PWD/Sequências_Brutas/ERS255960.fastq.gz   
ERS255961	$PWD/Sequências_Brutas/ERS255961.fastq.gz   
# Para sair, aperte Ctrl+X, depois S, coloque o nome do arquivo e ENTER  
```
**2) Importando para o Qiime** 

```coffeescript=
# Crie uma pasta 06.ReadsQza
# Agora você vai importar o ManifestFile para o Qiime:

# Sequências Paired-End 
qiime tools import --type 'SampleData[PairedEndSequencesWithQuality]' --input-path ManifestFile --output-path reads_trimmed.qza --input-format PairedEndFastqManifestPhred33V2

# Sequências Single-End
qiime tools import --type 'SampleData[SequencesWithQuality]' --input-path ManifestFile --output-path reads_trimmed.qza --input-format SingleEndFastqManifestPhred33V2

#Mova para a pasta 06.ReadsQza
mv ManifestFile 06.ReadsQza
mv reads_trimmed.qza 06.ReadsQza
```

---

## Passo 6: Criando uma tabela com o número de sequências em cada amostra

Para construir essa tabela (arquivo ***.tsv***), você precisa instalar o **Microbiome Helper**, um repositório de scripts que ajudam a processar e automatizar várias ferramentas bioinformáticas microbiomas e metagenômicas. 

**1) Instalando o Microbiome Helper**
```coffeescript= 
# Download
git clone https://github.com/LangilleLab/microbiome_helper.git 
# Use esse comando caso o git não esteja instalado e, depois, repita o comando anterior:
sudo apt install git 
```
**2) Construindo a tabela**
```coffeescript=
CAMINHO_PARA_PASTA_MICROBIOME_HELPER/qiime2_fastq_lengths.py 06.ReadsQza/reads_trimmed.qza --proc 4 -o 06.ReadsQza/read_counts.tsv 
# Para ver a tabela:
nano read_counts.tsv
# Para sair, digite Ctrl+X
# Mova o arquivo para a pasta 06.ReadsQza
```
Na tabela gerada, você encontrará na primeira coluna o nome de cada amostra inserida no ManifestFile e, na segunda coluna, o número de reads presentes em cada amostra. 

![](https://i.imgur.com/TcU6RlS.png)

---

## Passo 7: Qualidade com o DADA2

*O pacote dada2 é centrado no algoritmo DADA2 para obter alta resolução precisa da composição da amostra a partir de dados de sequenciamento de amplicons. O algoritmo DADA2 é mais sensível e mais específico do que os métodos OTU comumente usados e resolve variantes de sequência de amplicons (ASVs) que diferem em apenas um nucleotídeo.*

```coffeescript=
#Crie uma pasta
mkdir 07.DadaOutput
# Para determinar os parâmetros de remoção de erros ocorridos no processo de sequenciamento (denoising), precisam ser analizados os gráficos de qualidade do arquivo obtido no Passo 5, usando os códigos:
qiime demux summarize --i-data 06.ReadsQza/reads_trimmed.qza --o-visualization 06.ReadsQza/reads_trimmed.qzv
# Para observar os arquivos .qza ou .qsv, use o Qiime 2 View no site https://view.qiime2.org/

# Sequências Paired-End
# Vá mudando o nome da última pasta de output (no caso colocada no tutorial como NOVA_PASTA) pois, caso ela exista anteriormente, dará erro   
qiime dada2 denoise-paired --i-demultiplexed-seqs 06.ReadsQza/reads_trimmed.qza --p-trunc-len-f 0 --p-trunc-len-r 0 --output-dir 07.DadaOutput/NOVA_PASTA

# Sequências Single-End
# Vá mudando o nome da última pasta de output (no caso colocada no tutorial como NOVA_PASTA) pois, caso ela exista anteriormente, dará erro    
qiime dada2 denoise-single --i-demultiplexed-seqs 06.ReadsQza/reads_trimmed.qza --p-trunc-len 0 --output-dir 07.DadaOutput/NOVA_PASTA


# Para facilitar a observação da tabela, converta para .tsv
qiime tools export --input-path 07.DadaOutput/denoising_stats.qza --output-path 07.DadaOutput
```
A tabela criada possuirá 7 colunas:

**1º:** ID das amostras
**2º:** Número de reads input
**3º:** Número de reads output (ou seja, que sobreviveram à filtragem)
**4º:** A porcentagem de reads que foram filtrados 
**5º:** Número de reads das quais foram eliminados quaisquer erros no processo de sequenciamento
**6º:** Número de reads sem quimeras
**7º:** Porcentagem de reads sem quimeras comparado ao número de reads input 

---

## Passo 8: Árvore Filogenética

Além dos dados com boa qualidade, sem erros decorridos do sequenciamento, devidamente ajustados, essas sequências requerem uma árvore filogenética enraizada que faça a relação entre eles. 

```coffeescript=
#Crie uma pasta para colocar os dados da árvore
mkdir 08.TreeOut
```
++Inputs:++
**--i-alignment** 
Artefato FeatureData[AlignedSequence]: Sequências alinhadas a serem usadas na reconstrução filogenética . [obrigatório] 
      
++Parameters:++
**--p-n-threads** 
Valor Inteiro: O número de threads. O uso de mais de um thread executa a variante não determinística do *FastTree* (FastTreeMP) e pode resultar em uma árvore diferente da segmentação única. [padrão: 1]

++Outputs:++
**--o-alignment**
**--o-masked-alignment**
**--o-tree** 
**--o-rooted-tree**
Artefato: Nome do arquivo resultante. [Obrigatório]

++Diversos:++
**--output-dir** 
Caminho: Local de saída do arquivo resultante. [Não obrigatório]

O alinhamento multiplo das sequências é feito com o programa MAFFT, usando o seguinte código:

```coffeescript=
qiime alignment mafft --i-sequences 07.DadaOutput/representative_sequences.qza --p-n-threads 1 --o-alignment 08.TreeOut/rep_seqs_aligned.qza
```
Em seguida, o alinhamento é mascarado (ou filtrado) para remover posições altamente variáveis, evitando erros na árvore.
```coffeescript=
qiime alignment mask --i-alignment 08.TreeOut/rep_seqs_aligned.qza --o-masked-alignment 08.TreeOut/rep_seqs_aligned_masked.qza
```
Depois disso, o FastTree gerar uma árvore filogenética a partir do alinhamento mascarado.
```coffeescript=
qiime phylogeny fasttree --i-alignment 08.TreeOut/rep_seqs_aligned_masked.qza --p-n-threads 4 --o-tree 08.TreeOut/rep_seqs_aligned_masked_tree.qza
```
O programa FastTree cria uma árvore "sem raiz". Portanto, nesta etapa final, o enraizamento no ponto médio é aplicado para colocar a raiz da árvore no ponto médio da maior distância de ponta a ponta da árvore não enraizada.

```coffeescript=
qiime phylogeny midpoint-root --i-tree 08.TreeOut/rep_seqs_aligned_masked_tree.qza --o-rooted-tree 08.TreeOut/rep_seqs_aligned_masked_tree_rooted.qza
# Exportar a árvore para o Qiime
qiime tools export --input-path 08.TreeOut/rep_seqs_aligned_masked_tree_rooted.qza --output-path 08.TreeOut/exported-tree
```
Para visualizar a árvore, entre no [site](http://etetoolkit.org/treeview/) do ETE Toolkit. 

---

## Passo 9: Taxonomia 

Neste tutorial, será ensinado como identificar taxonomicamente suas amostras utilizando dois bancos de dados diferentes: **Greengenes** e **Silva**. Você pode fazer o download do database de sua preferência, ou treinar ele para uma região específica. 

**1) Download**

Para fazer o download de ambos, você pode entrar neste [site](https://docs.qiime2.org/2019.10/data-resources/) do Qiime 2. **Lembre-se de informar a sua versão do Qiime, seguindo a indicação da imagem a seguir:**

![](https://i.imgur.com/XRuiE6Q.png)


Para fazer o download dos databases já treinados:

![](https://i.imgur.com/2QX4cik.png)

**2) Treinar um database completo**

Para criar um database somente com a região que você precisa, é necessário que você possua os primers específicos que delimitam a mesma e o database completo. 
Digamos que você queira um arquivo da região V4, você precisará dos primers **515F**(GTGCCAGCMGCCGCGGTAA) e **806R**(GGACTACHVGGGTWTCTAAT). 

* **Greengenes:** 

Entre neste [site](http://blog.mothur.org/2014/08/12/greengenes-v13_8_99-reference-files/). Agora:
```coffeescript=
#Use o primeiro comando que é apresentado: 
wget -N ftp://greengenes.microbio.me/greengenes_release/gg_13_5/gg_13_8_otus.tar.gz

#Você vai obter um arquivo chamado gg_13_8_otus.tar.gz;
#Descompacte com o comando:
tar xvzf gg_13_8_otus.tar.gz
```
A pasta de saída terá o mesmo nome do arquivo anterior compactado. Dentro dela, duas pastas possuem os arquivos que você precisa: **rep_set** e **taxonomy**. 
Na pasta **rep_set**, recomendo que você use o arquivo *99_otu.fasta*, onde as sequências são mais ricas em informações, pois estão agrupadas com 99% de similaridade. 
Dessa forma, o arquivo que você vai importar para o Qiime2 se chama 99_otus.fasta.
```coffeescript=
#Transforme em .qza:
qiime tools import --type 'FeatureData[Sequence]' --input-path gg_13_8_otus/rep_set/99_otus.fasta --output-path gg_13_8_otus/99_otus.qza
```
Na pasta **taxonomy**, o arquivo que você vai usar se chama *99_otus_taxonomy.txt*, seguindo o critério do arquivo anterior. Como este arquivo é separado por tabulação (***.tsv***) sem cabeçalho, é necessário especificar ao Qiime o formato de origem do arquivo (**HeaderlessTSVTaxonomyFormat**), pois o formato de origem padrão para importação no Qiime2 requer um cabeçalho.
```coffeescript=
#Transforme em .qza:
qiime tools import --type 'FeatureData[Taxonomy]' --input-format HeaderlessTSVTaxonomyFormat --input-path gg_13_8_otus/taxonomy/99_otu_taxonomy.txt --output-path gg_13_8_otus/ref-taxonomy.qza
```
Os classificadores taxonômicos têm melhor desempenho quando treinados com base em seus parâmetros específicos de preparação e sequenciamento de amostras, incluindo os adaptadores que foram usados para amplificação das sequências. 
Então, extraia as reads do arquivo *99_otu.qza*, usando seus primers como referencial. Alguns comandos que você precisa alterar:

**--p-f-primer:** primer forward
**--p-r-primer:** primer reverse
**--p-trunc-len:**  Trunca as leituras que estão após o número bases estabelecida por você. Leituras mais curtas que esse valor são descartadas.
**!** Se as suas sequências forem do tipo paired-end, não é necessário usar este comando, pois suas leituras abrangem toda a região amplificada e, portanto, você pode extrair a leitura completa **!**  
**--p-min-lenght:** este parâmetro estabelece o tamanho mínimo da sequência (em bases)
**--p-max-lenght:** neste caso, é estabelecido o tamanho máximo da sequência (em bases)
```coffeescript=
qiime feature-classifier extract-reads --i-sequences gg_13_8_otus/99_otus.qza --p-f-primer GTGCCAGCMGCCGCGGTAA --p-r-primer GGACTACHVGGGTWTCTAAT --p-trunc-len 120 --p-min-length 100 --p-max-length 600 --o-reads ref-seqs.qza --verbose 
```
Agora você pode treinar um classificador usando as leituras de referência geradas no comando anterior (*ref-seqs.qza*) e a taxonomia de referência (*ref-taxonomy.qza*).
```coffeescript=
qiime feature-classifier fit-classifier-naive-bayes --i-reference-reads ref-seqs.qza --i-reference-taxonomy ref-taxonomy.qza --o-classifier greengenes_classifier_V4.qza
```
* **Silva**

O processo será semelhante ao usado com o Greengenes, porém, ele exigirá mais memória do seu computador. Dessa forma, caso ocorra algum erro ao longo do treinamento, possivelmente será por consequência desse fator. Por isso, recomendo que faça esse procedimento através de um servidor.
```coffeescript=
# Faça o download do database completo
wget https://www.arb-silva.de/fileadmin/silva_databases/qiime/Silva_132_release.zip

#Você vai obter um arquivo chamado Silva_132_release.zip;
# Agora é preciso descompactar
unzip Silva_132_release.zip
```
A pasta de saída que você precisará se chama **SILVA_132_QIIME_release**. Dentro dela, duas pastas possuem os arquivos que você precisa: **rep_set** e **taxonomy**. 
Na pasta **rep_set**, entre no diretório **rep_set_16S_only**. Recomendo que você use o arquivo da pasta **99**, chamado **silva_132_99_16S.fna**, pois, como no caso Greengenes, as sequências são mais ricas em informações, pois estão agrupadas com 99% de similaridade. 
Dessa forma, o arquivo que você vai importar para o Qiime2 se chama silva_132_99_16S.fna.
```coffeescript=
#Transforme em .qza:
qiime tools import --type 'FeatureData[Sequence]' --input-path SILVA_132_QIIME_release/rep_set/rep_set_16S_only/99/silva_132_99_16S.fna --output-path SILVA_132_QIIME_release/99_otus.qza
```
Na pasta **taxonomy**, entre no diretório **16S_only**, depois em **99**, seguindo o critério do arquivo anterior. Existem sete arquivos nesta pasta. O que será utilizado neste tutorial será o **taxonomy_all_levels.txt**, mas você pode escolher de acordo com sua preferência. 
Como este arquivo é separado por tabulação (***.tsv***) sem cabeçalho, é necessário especificar ao Qiime o formato de origem do arquivo (**HeaderlessTSVTaxonomyFormat**), pois o formato de origem padrão para importação no Qiime2 requer um cabeçalho.
```coffeescript=
#Transforme em .qza:
qiime tools import --type 'FeatureData[Taxonomy]' --input-format HeaderlessTSVTaxonomyFormat --input-path SILVA_132_QIIME_release/taxonomy/16S_only/99/taxonomy_all_levels.txt --output-path SILVA_132_QIIME_release/ref-taxonomy.qza
```

Agora extraia as reads do arquivo *99_otu.qza*, usando seus primers como referencial:
```coffeescript=
qiime feature-classifier extract-reads --i-sequences gg_13_8_otus/99_otus.qza --p-f-primer GTGCCAGCMGCCGCGGTAA --p-r-primer GGACTACHVGGGTWTCTAAT --p-trunc-len 120 --p-min-length 100 --p-max-length 600 --o-reads ref-seqs.qza --verbose 
```
Agora você pode treinar um classificador usando as leituras de referência geradas no comando anterior (*ref-seqs.qza*) e a taxonomia de referência (*ref-taxonomy.qza*).
```coffeescript=
qiime feature-classifier fit-classifier-naive-bayes --i-reference-reads ref-seqs.qza --i-reference-taxonomy ref-taxonomy.qza --o-classifier silva_classifier_V4.qza
```

**3) Gerando a taxonomia**

Agora, você vai começar a explorar a composição taxonômica das amostras. 

* **Greengenes**

A primeira etapa desse processo é atribuir classificação taxonômica às sequências no **FeatureData[Sequence]**, artefato do QIIME 2. Você fará isso usando um classificador pré-treinado (obtido pela internet, ou após o treinamento do database feito por você) e o *plugin* **q2-feature-classifier**. 

```coffeescript=
qiime feature-classifier classify-sklearn --i-reads 07.DadaOutput/representative_sequences.qza --i-classifier CLASSIFICADOR_GREENGENES.qza --o-classification taxonomy_greengenes.qza --verbose

#Movendo para uma pasta onde serão guardadas as taxonomias
#Primeiro crie a pasta
mkdir 09.TaxonomicAnalysis
#Agora mova o arquivo para ela
mv taxonomy_greengenes.qza 09.TaxonomicAnalysis
```

Agora vamos extrair o arquivo **table.qza** que foi criado durante o DADA2 e colocar em nova pasta chamada OtuTable, dentro da pasta 09.TaxonomicAnalysis. Em seguida, usando o comando *biom convert*, converta o arquivo **feature-table.biom**, que está dentro do **table.qza**, para ***.tsv***. Este arquivo contém nossa tabela de OTUs. Convertendo para ***.tsv*** conseguiremos lê-lo.

```coffeescript=
qiime tools export --input-path 07.DadaOutput/table.qza --output-path 09.TaxonomicAnalysis/OtuTable
#Convertendo para .tsv
biom convert -i 09.TaxonomicAnalysis/OtuTable/feature-table.biom -o 09.TaxonomicAnalysis/Otu_Table.tsv --to-tsv --table-type "OTU table"
```

A partir das classificações taxonômicas obtidas com o *plugin* **q2-feature-classifier**, os nomes das OTUs serão substituídos pelas taxonomias correspondentes em um novo arquivo.  

```coffeescript=
#Em --i-taxonomy, coloque o arquivo .qza que foi gerado após a taxonomia
#Em --p-level são designados os graus de classificação taxonômica
#Como saída, você obterá um arquivo chamado CollapsedTable, em formato .qza
qiime taxa collapse --i-table 07.DadaOutput/table.qza --i-taxonomy 09.TaxonomicAnalysis/taxonomy_greengenes.qza --p-level 7 --o-collapsed-table 09.TaxonomicAnalysis/CollapsedTable
#Extraia o arquivo feature-table.biom que está dentro de CollapsedTable.qza 
qiime tools export --input-path 09.TaxonomicAnalysis/CollapsedTable.qza --output-path 09.TaxonomicAnalysis
#Converta para .tsv
biom convert -i 09.TaxonomicAnalysis/feature-table.biom -o 09.TaxonomicAnalysis/CollapsedTable.tsv --to-tsv --table-type "OTU table"
```

Por fim, extraia a tabela de taxonomia que está dentro do arquivo **taxonomy.qza** gerada após a comparação com o banco de dados que você baixou ou treinou.   

```coffeescript=
qiime tools export --input-path  09.TaxonomicAnalysis/taxonomy_greengenes.qza --output-path 09.TaxonomicAnalysis
```

* **Silva**

Os passos para a realizar a taxonomia com o database Silva são os mesmos utilizados com o Greengenes: 
```coffeescript=
#Baixar o database Silva V3-V4 no site https://forum.qiime2.org/t/looking-for-classifier-silva-v3-v4-generated-with-scikit-learn-version-0-21-2/11135
#Atribuir classificação taxonômica às sequências
qiime feature-classifier classify-sklearn --i-classifier /REFERENCE/genomes/rlappan/SILVA/SILVA_132_QIIME_release/classifier_16S_V3-V4.qza --i-reads ../DADA2_denoising_output/representative_sequences.qza --o-classification taxonomy_silva.qza

#Movendo para uma pasta onde serão guardadas as taxonomias
#Primeiro crie a pasta
mkdir 09.TaxonomicAnalysis
#Agora mova o arquivo para ela
mv taxonomy_silva.qza 09.TaxonomicAnalysis

#Extraia o arquivo table.qza que foi criado durante o DADA2 e coloque nesta nova pasta
qiime tools export --input-path 07.DadaOutput/table.qza --output-path 09.TaxonomicAnalysis/OtuTable
#Convertendo para .tsv
biom convert -i 09.TaxonomicAnalysis/OtuTable/feature-table.biom -o 09.TaxonomicAnalysis/Otu_Table.tsv --to-tsv --table-type "OTU table"

#Em --i-taxonomy, coloque o arquivo .qza que foi gerado após a taxonomia
#Em --p-level são designados os graus de classificação taxonômica
#Como saída, você obterá um arquivo chamado CollapsedTable, em formato .qza
qiime taxa collapse --i-table 07.DadaOutput/table.qza --i-taxonomy 09.TaxonomicAnalysis/taxonomy_silva.qza --p-level 7 --o-collapsed-table 09.TaxonomicAnalysis/CollapsedTable

#Extraia o arquivo feature-table.biom que está dentro de CollapsedTable.qza 
qiime tools export --input-path 09.TaxonomicAnalysis/CollapsedTable.qza --output-path 09.TaxonomicAnalysis

#Converta para .tsv
biom convert -i 11.TaxonomicAnalysis/feature-table.biom -o 11.TaxonomicAnalysis/CollapsedTable.tsv --to-tsv --table-type "OTU table"

#Extraia a tabela de taxonomia que está dentro do arquivo taxonomy.qza
qiime tools export --input-path  11.TaxonomicAnalysis/SilvaTaxonomy.qza --output-path 11.TaxonomicAnalysis
```