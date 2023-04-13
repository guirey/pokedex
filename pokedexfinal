import pandas as pd
import json as js
import PySimpleGUI as sg
import tabulate

out = 1
while out == 1:

    font = ("Consolas", 12)  # define a fonte padrao como consolas (monoespaçada)
    sg.set_options(font=font)
    sg.theme('Green Mono')  # definindo o tema das janelas

    tabela = pd.read_excel("base.xlsx")  # importa a base de dados
    tabela = tabela.drop("photo", axis=1)  # remove a coluna photo (axis = 1 coluna, axis = 0 linha)
    tabela['name'] = tabela['name'].str.lower()  # transforma o nome dos pokemons tudo em minusculo

    layout = [[sg.Text('Contra qual pokemón você está?')],
              [sg.InputText()],
              [sg.Submit('buscar'), sg.Cancel('num kero')]]

    window = sg.Window('Pokedex 2.0', layout)

    event, values = window.read()
    window.close()

    poke = values[0]
    encontrado = (tabela.loc[tabela['name'] == poke])  # encontra o pokemon na base

    if encontrado.empty == True and event != sg.WIN_CLOSED and event != 'num kero':  # faz a validação se a entrada lida do usario é valida
        while encontrado.empty == True:  # valida até ser possivel encontrar na base
            layout = [[sg.Text('Por favor, insira corretamente o nome:')],
                      [sg.InputText()],
                      [sg.Submit('buscar'), sg.Cancel('num kero')]]

            window = sg.Window('Pokedex 2.0', layout, keep_on_top=True, grab_anywhere=True)
            event, values = window.read()
            window.close()
            poke = values[0]
            encontrado = (tabela.loc[tabela['name'] == poke])  # encontra o pokemon na base
            if event == sg.WIN_CLOSED or event == 'num kero':
                out = 0
                break
                window.close()

    NumeroDoPoke = encontrado.iloc[0, 0]
    NomeDoPoke = encontrado.iloc[0, 1]
    Tipo1 = encontrado.iloc[0, 2]
    Tipo2 = encontrado.iloc[0, 3]

    if pd.isna(Tipo2) == True:  # verifica se o valor é vazio, se o Tipo 2 for vazio, cria uma tupla sem ele
        TuplaPoke = (NumeroDoPoke, NomeDoPoke, Tipo1)
    else:  # se o Tipo 2 for preenchido, cria uma tupla com ele
        TuplaPoke = (NumeroDoPoke, NomeDoPoke, Tipo1, Tipo2)

    # quanto a defesa, fracos:
    tipos = """{"Voador":["Eletrico", "Gelo", "Pedra"], "Planta":["Fogo", "Gelo", "Venenoso", "Voador", "Inseto"],"Fogo":["Agua", "Terrestre", "Pedra"],"Agua":["Eletrico", "Planta"],"Inseto":["Fogo", "Voador", "Pedra"],"Normal":["Lutador"],"Venenoso":["Terrestre", "Psiquico"],"Eletrico":["Terrestre"],"Terrestre":["Agua", "Planta", "Gelo"],"Fada":["Venenoso", "Aco"],"Lutador":["Voador", "Psiquico", "Fada"],"Psiquico":["Inseto", "Fantasma", "Sombrio"],"Pedra":["Agua", "Planta", "Lutador", "Terrestre", "Aco"],"Fantasma":["Fantasma", "Sombrio"],"Gelo":["Fogo", "Lutador", "Pedra", "Aco"],"Dragao":["Gelo", "Dragao", "Fada"],"Aco":["Fogo", "Lutador", "Terrestre"],"Sombrio":["Lutador", "Inseto", "Fada"]}"""
    tipos = js.loads(tipos)

    # quanto a defesa, fortes:
    TiposDef = """{"Voador":["Terrestre", "Lutador", "Inseto", "Planta"], "Planta": ["Agua", "Eletrico", "Planta", "Terrestre"], "Fogo": ["Fogo", "Gelo", "Planta", "Inseto", "Aco", "Fada"], "Agua": ["Fogo", "Agua", "Gelo", "Aco"], "Inseto": ["Lutador", "Planta", "Terrestre"], "Normal": ["Fantasma"], "Venenoso": ["Lutador", "Venenoso", "Planta", "Inseto", "Fada"], "Eletrico": ["Voador", "Eletrico", "Aco"], "Terrestre": ["Venenoso", "Eletrico", "Pedra"], "Fada": ["Dragao", "Lutador", "Inseto", "Sombrio"], "Lutador": ["Inseto", "Pedra", "Sombrio"], "Psiquico": ["Lutador", "Psiquico"], "Pedra": ["Normal", "Fogo", "Venenoso", "Voador"], "Fantasma": ["Normal", "Lutador", "Venenoso", "Inseto"], "Gelo": ["Gelo"], "Dragao": ["Agua", "Eletrico", "Planta", "Fogo"], "Aco": ["Normal", "Planta", "Gelo", "Voador", "Psiquico", "Inseto","Pedra","Dragao", "Aco","Fada"], "Sombrio": ["Psiquico", "Fantasma", "Sombrio"]}"""
    TiposDef = js.loads(TiposDef)

    QuantosTipos = len(TuplaPoke)

    if QuantosTipos == 3:  # se o pokemon possui apenas 1 tipo, suas fraquezas sao retornadas direto
        ResultadoFraquezas = tipos[TuplaPoke[2]]

    else:  # caso ele possua dois tipos
        PrimeiraFraqueza = tipos[TuplaPoke[2]]  # a primeira fraqueza é o resultado do tipo 1
        SegundaFraqueza = tipos[TuplaPoke[3]]  # a segunda é o resultado do tipo 2
        fraqueza = PrimeiraFraqueza + SegundaFraqueza  # a soma entre as duas
        ListaFraquezas = list(set(fraqueza))  # apenas os resultados unicos são retornados a fim de evitar duplicaçao

        # comparar ListaForcas com ListaFraquezas para encontrar o resultado

        PrimeiraForca = TiposDef[TuplaPoke[2]]  # a primeira fraqueza é o resultado do tipo 1
        SegundaForca = TiposDef[TuplaPoke[3]]  # a segunda é o resultado do tipo 2
        forca = PrimeiraForca + SegundaForca  # a soma entre as duas
        ListaForcas = list(set(forca))  # apenas os resultados unicos são retornados a fim de evitar duplicaçao

        # exclui itens de fraqueza que estão em força, deixando apenas uma lista com fraquezas
        ResultadoFraquezas = []
        for f in ListaFraquezas:
            if f not in ListaForcas:
                ResultadoFraquezas.append(f)

    TextoFraq = f"""\nO pokemón {poke.capitalize()} é fraco contra os seguintes tipos:
{ResultadoFraquezas}\n"""  # o f na frente das aspas triplas para poder ler as variaveis

    tabela.type2 = tabela.type2.fillna('-------')  # substitui os valores vazios no dataframe por string -

    # FortesContra = []
    FortesContraNovo = pd.DataFrame({})

    # fazer um sort na base de dados com os componentes da lista ResultadoFraquezas
    # if FortesContra.empty==True:                                   #verifica se a primeira resposta é vazia
    for i in range(len(ResultadoFraquezas)):
        FortesContra = (tabela.loc[tabela['type1'] == ResultadoFraquezas[
            i]])  # pega na base pokemons que sejam do tipo igual a primeira fraqueza
        FortesContraNovo = pd.concat([FortesContra, FortesContraNovo])  # concatena todos os tipos encontrados em um so

    ResultadoFinal = FortesContraNovo.sample(n=10)  # pega resultados aleatorios do dataframe
    # FortesContraSliceSemIndex = FortesContraSlice.to_string(index=False) #tira os indices automaticos da tabela
    ResultadoFinalGrid = ResultadoFinal.to_markdown(tablefmt="grid")  # insere uma grade no dataframe

    TextoFortes = f"""\nAlguns pokemóns que são fortes contra {poke.capitalize()}:
{ResultadoFinalGrid}"""  # o f na frente das aspas triplas para poder ler as variaveis

    event, values = sg.Window('Pokefracos',
                              [[sg.T(f'{TextoFraq}{TextoFortes}', key='-txt-')],
                               [sg.B('outro', key='-1-', bind_return_key=True)],
                               [sg.B('refresh')],
                               [sg.Cancel('sair')]], keep_on_top=True, grab_anywhere=True,
                              element_justification='c').read(
        close=True)  # utilizando window para printar e nao popup para poder centralizar os elementos na tela

    if event == sg.WIN_CLOSED or event == 'sair':  # If user closed window with X or if user clicked "Exit" button then exit
        out = 0
        break

    elif event == 'outro':
        out = 1

    elif event == 'refresh':
        while event != 'outro' or event != sg.WIN_CLOSED or event != 'sair':
            ResultadoFinal = FortesContraNovo.sample(n=10)
            ResultadoFinalGrid = ResultadoFinal.to_markdown(tablefmt="grid")
            event, values = sg.Window('Pokefracos',
                                      [[sg.T(
                                          f'Outros pokemóns que são fortes contra {poke.capitalize()}:\n{ResultadoFinalGrid}')],
                                       [sg.B('refresh')],
                                       [sg.Cancel('sair')]], keep_on_top=True, grab_anywhere=True,
                                      element_justification='c').read(close=True)

            if event == sg.WIN_CLOSED or event == 'sair':  # If user closed window with X or if user clicked "Exit" button then exit
                out = 0
                break

    window.close()
