# atividade_API
import requests
import pandas as pd

# etapa 1
url = 'https://api.open-meteo.com/v1/forecast'

lat = float(input('Informe a latitude desejada '))
lon = float(input('Informe a longitude desejada '))
quantos_dias = int(input('Informe quantos dias '))
if quantos_dias > 7:
  quantos_dias = 7
  print('Vizualização de até 7 dias ')

parametros = {
    'latitude' : lat,
    'longitude' : lon,
    'daily' : ['temperature_2m_max','precipitation_sum'],
    'timezone' : 'auto',
    'past_days' : quantos_dias
}
# etapa 2
resposta = requests.get(url, params=parametros)
if resposta.status_code==200:
  dados = resposta.json()
  dados_diarios = dados['daily']
  df_dados_clima = pd.DataFrame(dados_diarios)
  df_dados_clima = df_dados_clima.head(quantos_dias)
  display(df_dados_clima)

# Etapa 3
  df_dados_clima.to_csv('atv-prof-luyz.csv', index=False)
else:
  print(f'Erro ao exibir API {resposta.status_code}')
