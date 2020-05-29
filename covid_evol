# -*- coding: utf-8 -*-
"""
Created on Tue May 19 23:26:40 2020

@author: Monin
"""
"""
import streamlit as st
import pandas as pd
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib.ticker as ticker
import matplotlib.animation as animation
from IPython.display import HTML

#First we get data from one of the oficial goverment repositories (MinCiencia github repo)

df_region_raw = pd.read_csv("https://raw.githubusercontent.com/MinCiencia/Datos-COVID19/master/output/producto3/CasosTotalesCumulativo.csv")

df_region = pd.DataFrame(columns=['ndia','fecha', 'region','confirmados'])
ind=0
counter = 1
for j in df_region_raw.iloc[:,1:].columns: 
    for i in df_region_raw.index:
        df_region.loc[ind] = [counter, j, df_region_raw.iloc[i,0], df_region_raw.loc[i,j] ]   # adding a row
        ind += 1
    counter += 1
df_region = df_region[(df_region.region !="Total")]

# Assign "Zone" to our data
def zonas(row):
    if row in ['Arica y Parinacota', 'Tarapacá', 'Antofagasta', 'Atacama', 'Coquimbo']:
        val = 'Zona Norte'
    elif row in ['Valparaíso', 'Metropolitana', 'O’Higgins', 'Maule']:
        val = 'Zona Centro'
    else:
        val = 'Zona Sur'
    return val

df_region['zona'] = df_region.region.apply(zonas)
colors = dict(zip(
    ['Zona Norte', 'Zona Centro', 'Zona Sur'],
    ['#adb0ff', '#ffb3ff', '#90d595'] ))

group_lk = df_region.set_index('region')['zona'].to_dict()

#Create a function to generates de evolution 

fig, ax = plt.subplots(figsize=(15, 8))

def draw_barchart(current_day):
    dff = df_region[df_region['ndia'].eq(current_day)].sort_values(by='confirmados', ascending=True).tail(10)
    ax.clear()
    ax.barh(dff['region'], dff['confirmados'], color=[colors[group_lk[x]] for x in dff['region']])
    dx = dff['confirmados'].max() / 200
    for i, (value, name) in enumerate(zip(dff['confirmados'], dff['region'])):
        ax.text(value-dx, i,     name,           size=14, weight=600, ha='right', va='bottom')
        ax.text(value-dx, i-.25, group_lk[name], size=10, color='#444444', ha='right', va='baseline')
        ax.text(value+dx, i,     f'{value:,.0f}',  size=14, ha='left',  va='center')
    ax.text(1, 0.4, current_day, transform=ax.transAxes, color='#777777', size=46, ha='right', weight=800)
    ax.text(0, 1.06, 'Confirmados', transform=ax.transAxes, size=12, color='#777777')
    ax.xaxis.set_major_formatter(ticker.StrMethodFormatter('{x:,.0f}'))
    ax.xaxis.set_ticks_position('top')
    ax.tick_params(axis='x', colors='#777777', labelsize=12)
    ax.set_yticks([])
    ax.margins(0, 0.01)
    ax.grid(which='major', axis='x', linestyle='-')
    ax.set_axisbelow(True)
    ax.text(0, 1.15, 'Casos confirmados por región covid Chile',
            transform=ax.transAxes, size=24, weight=600, ha='left', va='top')
    ax.text(1, 0, 'by Monin Leonor @joquitz; credit @pratapvardhan', transform=ax.transAxes, color='#777777', ha='right',
            bbox=dict(facecolor='white', alpha=0.8, edgecolor='white'))
    plt.box(False)


# Create a simple app 
ultimodia = df_region.ndia.max()
st.title("Evolucion del Covid19 en Chile por region")
st.markdown("Chile 2020 luego de %i" % ultimodia)

@st.cache(persist=True)
fig, ax = plt.subplots(figsize=(15, 8))
animator = animation.FuncAnimation(fig, draw_barchart, frames= range(1,ultimodia))
st.write(HTML(animator.to_jshtml()))
