# Jogo das portas
Você está num programa de televisão onde deve escolher uma porta entre três, em apenas uma delas há um premio. Vamos chamar as portas de 1, 2 e 3. Você escolhe a porta 1, então o apresentar abre a porta 2 onde não há premio algum, logo o premio está na porta 1 ou 3.

Agora vem a questão: Você deve mudar de porta? Alguns dizem que: quando a primeira porta é escolhida existem 3 opções, logo a probabilidade de escolher a porta correta é 1/3, já, mudando de porta após a abertura da primeira, existem 2 portas e então a probabilidade de escolher a porta correta é 1/2. E agora? Bora rodar simulações de Monte Carlo pra ver o que acontece.

Importando as bibliotecas


```python
import numpy as np
```

Construindo uma função para fazer isso com parametros:
* Ns: Número de simulações por ciclo
* Ncicl: Número de ciclos
* MudaP: Mudar de porta?

Agora temos o seguinte: mantendo a primeira porta basta sortear qual das três terá o premio, qual é escolhida e ver o resultado. 
Agora mudando a porta temos o seguinte: como o "apresentador" mostra sempre uma porta que não tem premio e não foi escolhida ele faz com que os resultados possíveis sejam: certo ou errado, com isso, se a porta é alterada temos que o certo vira errado e vice versa pois só temos duas opções!


```python
def Simula(Ns=1000, Ncicl=10, MudaP=False):
    # Inicia acertos como 0
    acertos = 0

    # Roda ciclos
    for i in range(1, Ncicl-1):
        # Gera portas escolhidas e premiadas
        porta = np.random.randint(low=1, high=3+1, size=Ns)
        premio = np.random.randint(low=1, high=3+1, size=Ns)

        # Mudança de porta
        if MudaP == False:
            # Sem mudar porta: basta verificar
            # Se está correto seta 1, se não é 0
            testa = np.where(porta == premio, 1, 0)
        else:
            # Abre terceira porta que é errada, sobram apenas duas,
            # logo, se estava errado agora está certo e vice versa.
            testa = np.where(porta != premio, 1, 0)

        # Resultado é a soma do array
        acertos += np.sum(testa)
    
    # Determina taxa de acertos
    taxa = acertos/(Ncicl*Ns) 

    # Printa resultados
    return print("Taxa de acertos com MudaP={} e {:1.3e} simulações: {:.3f}.".format(MudaP,Ncicl*Ns,taxa))
```


```python
Simula(Ns=5000, Ncicl=10000, MudaP=False)
Simula(Ns=5000, Ncicl=10000, MudaP=True)
```

    Taxa de acertos com MudaP=False e 5.000e+07 simulações: 0.333.
    Taxa de acertos com MudaP=True e 5.000e+07 simulações: 0.667.
    

### Qual sentido disso?
Aparentemente isso está relacionado ao Teorema de Bayes $P(A|B)=\frac{P(B|A)P(A)}{P(B)}$ que pode ser reescrito pela lei da probabilidade total como 
$P(A)=\sum_{i=1}^n{P(A\cap B_i)}$ ou $P(A)=\sum_{i=1}^n{P(A|B_i)P(B_i)}$.

Inicialmente temos 3 opções de portas (A, B, C) com mesma probabilidade de acerto (1/3). Com o avanço do jogo uma porta não premiada e não escolhida é aberta, alterando a distribuição do problema. 

E agora?

