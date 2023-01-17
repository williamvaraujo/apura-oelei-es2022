# Projeto Final - Disciplina Análise Exploradora de Dados- Insper 2022 

:pencil: Passo-a-passo do projeto final da disciplina de Análise Exploradora de Dados, do Master em Jornalismo de Dados, Automação e Data Storytelling do Insper. A tarefa consiste em  escolher um conjunto de dados e explorá-los, estabelecendo questionamentos sobre eles e possíveis relações entre as variáveis. O convite foi de explorarmos os dados e gerarmos algumas hipóteses sobre eles. 

**Integrantes:**
- Ingrid Fernandes - [@indgris](https://github.com/indgris)
- João Leite - [@joaogpleite](https://github.com/joaogpleite)
- Karina Ferreira - [@karina-ferreira](https://github.com/karina-ferreira)
- Will Araújo - [@will](https://github.com/link)

:unlock: Vamos lá:


## :dart: Escolha

Escolhemos o banco de dados da Secretaria de Segurança Pública do Estado de São Paulo, com informações sobre as ocorrências registradas por cidade, entre os anos de 2002 e 2021. Os dados estão disponíveis no [basededados.org](https://basedosdados.org/dataset/br-sp-gov-ssp?bdm_table=ocorrencias_registradas), e o dicionário pode ser acessado lá mesmo. 

## :point_up: Perguntas
Ao subir os dados para o *Orange*, abrimos um *data table* para fazer a primeira visualização dos dados. O primeiro passo foi entender a tabela, com ajuda do dicionário. Passamos então a pensar em duas modalidades de perguntas: as que vinham à mente, e as que poderiam ser respondidas com os dados que disponíveis. Fizemos algumas visualizações usando ferramentas do *Orange* como *group by*, *box plot*, *distribution*, etc. Demos match nas perguntas das duas cateorias e chegamos à seguinte lista:

1. Qual tipo de ocorrência mais se repete?
2. Qual tipo de ocorrência mais se repete em cada ano da série histórica (2002-2021)?
3. Quais cidades registraram maior número de ocorrências? São elas sempre as mesmas na série histórica?
4. Há crimes diminuindo em quantidade e outros aumentando ao longo dos anos? Quais?
5. Existem meses com maior número de registros de ocorrências?
6. Existe ano com ausência de registro para alguma modalidade de crime?
7. Existem manchas de criminalidade em cidades próximas?
8. Qual a modalidade de homicídio que mais tem registros?
9. A mancha de criminalidade aumentou em uma cidade em detrimento de outra próxima?
10. Existem missing datas? Como são descritos?
11. Na evolução dos anos, o número de vítimas fatais diminuiu diante da quantidade de homicídios, se manteve constante ou aumentou enquanto a soma de registros da modalidade diminuiu?
12. Qual a moda entre os anos?
13. A partir de que ano cada tipo de ocorrência começou ser catalogada?

## :put_litter_in_its_place: Limpeza
Embora grandes entusiastas do *Orange*, podemos dizer que a paixão foi apagando conforme fomos apresentados à outras possibilidades de interação com os dados. O queridinho da vez é o *DB Browser*, onde utilizamos a linguagem *SQL* para construir *queries* que respondessem nossas perguntas. O primeiro e único passo da limpeza foi classificarmos as colunas que, para a maioria das perguntas, não seriam necessárias serem analisadas, como as que indicavam número de vítimas, ou as que somavam outras duas ou mais categorias. Para isso, fizemos a seuinte *querie*:

`colar o código aqui`

#1 - Importação das bibliotecas necessárias

import pandas as pd #esta biblioteca servirá para limpar as tabelas e consolidar os dados
from selenium import webdriver #esta biblioteca serve para comandar o navegador por meio de um bot
from webdriver_manager.chrome import ChromeDriverManager #esta biblioteca serve para atualizar o controlador do navegador
from selenium.webdriver.chrome.service import Service #esta biblioteca serve para atualizar o serviço automaticamente
from bs4 import BeautifulSoup
import os
import zipfile as zp
import altair as alt
import numpy as np
import missingno as msno
import re
import unidecode
import unicodedata
import pandasql as ps

#2 - Criando o controlador do navegador e fazendo com que atualize automaticamente

servico = Service(ChromeDriverManager().install())
navegador = webdriver.Chrome(service=servico)

#3 - Raspando as tabelas do TSE
    #Tabela candidatos
site = 'https://dadosabertos.tse.jus.br/dataset/?q=2022'
navegador.get(site)
navegador.find_element('xpath','//*[@id="content"]/div[3]/div/section[1]/div/ul/li[11]/div/h2/a').click()
navegador.find_element('xpath','//*[@id="dataset-resources"]/ul/li[1]/a').click()
navegador.find_element('xpath','//*[@id="content"]/div[3]/section/div/p/a').click()

    #Tabela Prestação de contas
navegador.get(site)
navegador.find_element('xpath','//*[@id="content"]/div[3]/div/section[1]/div/ul/li[9]/div/h2/a').click()
navegador.find_element('xpath','//*[@id="dataset-resources"]/ul/li[2]/a').click()
navegador.find_element('xpath','//*[@id="content"]/div[3]/section/div/p/a').click()


#4 - mudando de lugar os arquivos baixados e fazendo upload deles
#os.listdir(caminho_do_download) - execute este comando para saber o nome dos arquivos da pasta
caminho_do_download = r'C:\Users\Will Araújo\Downloads'
caminho_de_armazenamento = r'C:\Users\Will Araújo\Desktop\Python Jupyther'
os.rename(f'{caminho_do_download}/prestacao_de_contas_eleitorais_candidatos_2022.zip',f'{caminho_de_armazenamento}/prestacao_de_contas_eleitorais_candidatos_2022.zip')
os.rename(f'{caminho_do_download}/consulta_cand_2022.zip',f'{caminho_de_armazenamento}/consulta_cand_2022.zip')

#5 - Descompactando e abrindo o arquivo baixado
#zp.ZipFile('consulta_cand_2022.zip').namelist()
zp.ZipFile('consulta_cand_2022.zip').extract('consulta_cand_2022_BRASIL.csv')

#zp.ZipFile('prestacao_de_contas_eleitorais_candidatos_2022.zip').namelist()
zp.ZipFile('prestacao_de_contas_eleitorais_candidatos_2022.zip').extract('receitas_candidatos_2022_BRASIL.csv')

#6 - Abrindo as planilhas gerais e limpando com o Pandas (pd).
#Para isso, precisamos transformar os csvs em DataFrame (objetos do Pandas)






tabela_candidatos = pd.read_csv('consulta_cand_2022_BRASIL.csv',
                                sep=';',encoding='ISO-8859-1',
                                low_memory=False).query(' NR_CPF_CANDIDATO != -4  & NR_TURNO == 1')

tabela_candidatos.groupby('NR_CPF_CANDIDATO')

tabela_contas = pd.read_csv('receitas_candidatos_2022_BRASIL.csv',
                            sep=';', encoding='ISO-8859-1', 
                            low_memory=False).query(
    'DS_ORIGEM_RECEITA == "Recursos de outros candidatos" & DS_FONTE_RECEITA =="FUNDO ESPECIAL"')


#tabela_contas







#7 - Selecionando apenas algumas colunas antes do merge
tb_contas = tabela_contas[['VR_RECEITA',
                           'DS_RECEITA',
                           'SQ_CANDIDATO_DOADOR',
                           'NR_CPF_CNPJ_DOADOR',
                           'NR_CANDIDATO_DOADOR',
                           'NM_DOADOR',
                           'CD_CARGO_CANDIDATO_DOADOR',
                           'DS_CARGO_CANDIDATO_DOADOR',
                           'SG_PARTIDO_DOADOR',
                           'TP_PRESTACAO_CONTAS',
                           'SG_UE',
                           'SG_UF_DOADOR',
                           'DS_CARGO',
                           'CD_CARGO',
                           'SQ_CANDIDATO',
                           'NR_CANDIDATO',
                           'NM_CANDIDATO',
                           'NR_CPF_CANDIDATO',
                           'SG_PARTIDO',
                           'DS_FONTE_RECEITA',
                           'DS_ORIGEM_RECEITA',
                           'DS_NATUREZA_RECEITA',
                           'DS_ESPECIE_RECEITA',
                           'NR_RECIBO_DOACAO',
                           'NR_DOCUMENTO_DOACAO']]

tb_candidatos = tabela_candidatos[['SQ_CANDIDATO',
                                   'NR_CANDIDATO',
                                   'NM_CANDIDATO',
                                   'NR_CPF_CANDIDATO',
                                   'DS_GENERO',
                                   'DS_COR_RACA',
                                   'CD_CARGO']]



#8 - Fazendo merge e unindo as tabelas para candidatos BENEFICIADOS

tb_receitas_cor_genero_candidato = pd.merge(tb_contas,tb_candidatos,how='left',
                                            left_on= ['SQ_CANDIDATO'],
                                            right_on=['SQ_CANDIDATO'])
tb_receitas_cor_genero_candidato



#9 - Fazendo merge e unindo as tabelas para candidatos DOADORES

tb_comp = pd.merge(tb_receitas_cor_genero_candidato,tb_candidatos,how='left', left_on = ['SQ_CANDIDATO_DOADOR',
                                                                         'NR_CANDIDATO_DOADOR',
                                                                         'CD_CARGO_CANDIDATO_DOADOR'],
                             right_on=['SQ_CANDIDATO',
                                       'NR_CANDIDATO',
                                       'CD_CARGO'],
                             suffixes=('_contas', '_cand'))
tb_comp.keys()

#10 - Renomeando as colunas para melhorar o entendimento
tb_comp_renomeada = tb_comp.rename(columns={'VR_RECEITA':'doacao',
            'DS_RECEITA':'intencao',
            'SQ_CANDIDATO_DOADOR':'cod_doador',
            'NR_CPF_CNPJ_DOADOR':'cnpj_cpf_doador',
            'NR_CANDIDATO_DOADOR':'nr_doador',
            'NM_DOADOR':'nome_doador',
            'DS_GENERO_cand':'gen_doador',
            'DS_COR_RACA_cand':'cor_doador',
            'DS_CARGO_CANDIDATO_DOADOR':'cargo_doador',
            'SG_PARTIDO_DOADOR':'partido_doador',
            'SG_UF_DOADOR':'estado_doador',
            'TP_PRESTACAO_CONTAS':'tipo_prestacao_contas',
            'SQ_CANDIDATO_contas':'sq_beneficiado',
            'NR_CANDIDATO_x':'nr_beneficiado',
            'NR_CPF_CANDIDATO_x':'nr_cpf_beneficiado',
            'NM_CANDIDATO_x':'nome_beneficiado',
            'DS_GENERO_contas':'gen_beneficiado',
            'DS_COR_RACA_contas':'cor_beneficiado',
            'DS_CARGO':'cargo_beneficiado',
            'SG_PARTIDO':'partido_beneficiado',
            'SG_UE':'estado_beneficiado',
            'DS_FONTE_RECEITA':'fonte_receita',
            'DS_ORIGEM_RECEITA':'origem_valor',
            'DS_NATUREZA_RECEITA':'natureza_valor',
            'DS_ESPECIE_RECEITA':'especie_receita',
            'NR_RECIBO_DOACAO':'recibo_doacao',
            'NR_DOCUMENTO_DOACAO':'cod_doc_doacao'
                                             })
tb_comp_renomeada

#ESCOLHENDO AS COLUNAS

tb_total_sem_limpar = tb_comp_renomeada[['doacao',
            'intencao',
            'cod_doador',
            'cnpj_cpf_doador',
            'nr_doador',
            'nome_doador',
            'gen_doador',
            'cor_doador',
            'cargo_doador',
            'partido_doador',
            'tipo_prestacao_contas',
            'sq_beneficiado',
            'nr_beneficiado',
            'nr_cpf_beneficiado',
            'nome_beneficiado',
            'gen_beneficiado',
            'cor_beneficiado',
            'cargo_beneficiado',
            'partido_beneficiado',
            'fonte_receita',
            'origem_valor',
            'natureza_valor',
            'especie_receita',
            'recibo_doacao',
            'cod_doc_doacao']]

tb_total_sem_limpar

#EXPORTANDO PARA LIMPAR NO LADO DE FORA

tb_total_sem_limpar.to_csv('receitas_cor_gen_incompleta.csv',index=False)

(Notebook by WIll Araújo)

## :bar_chart: Visualização

Texto texto.

## :bulb: Hipóteses

Texto texto.
