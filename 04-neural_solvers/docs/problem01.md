## Equação do Calor 1D Transiente

Imagine uma barra metálica fina e longa, isolada em suas laterais. Queremos modelar como a temperatura $u$ varia ao longo do comprimento da barra ($x$) e com o tempo ($t$).

A física é descrita pela seguinte Equação Diferencial Parcial (EDP):

$$
\frac{\partial u}{\partial t} = \alpha \frac{\partial^2 u}{\partial x^2}
$$

Onde:
* $u(x,t)$ é a temperatura na posição $x$ e no tempo $t$.
* $\alpha$ é a difusividade térmica do material (uma constante).

Para resolver este problema, precisamos de:
* **Condição Inicial (CI):** A distribuição de temperatura na barra no início ($t=0$). Exemplo: uma onda senoidal.

$$ u(x, 0) = \sin(\pi x) $$
    
* **Condições de Contorno (CC):** O que acontece nas extremidades da barra (em $x=0$ e $x=L$). Exemplo: as pontas são mantidas em temperatura zero.

$$ u(0, t) = 0 \quad \text{e} \quad u(L, t) = 0 $$

## Por que este é o problema mais simples?

* **Baixa Dimensionalidade:** O input da rede neural é de apenas duas dimensões ($x,t$), e a saída é unidimensional ($u$). Isso torna a rede pequena, rápida de treinar e fácil de visualizar.
    
* **Linearidade:** A equação é linear. Não há termos como $u^2$ ou $u \frac{\partial u}{\partial x}$ (como na Equação de Burgers). Redes neurais tendem a aprender soluções para EDPs lineares com mais facilidade.
    
* **Derivadas Simples:** Envolve apenas uma derivada de primeira ordem no tempo e uma de segunda ordem no espaço. É o mínimo para um problema de evolução parabólico e introduz os conceitos fundamentais.
    
* **Física Intuitiva:** O fenômeno da difusão de calor é fácil de entender. Você espera que picos de temperatura se suavizem e se espalhem com o tempo, o que ajuda a verificar se a solução da PINN faz sentido visualmente.
    
* **Existência de Solução Analítica:** Para condições de contorno e iniciais simples (como as do exemplo), existe uma solução exata. Isso é extremamente valioso para iniciantes, pois você pode comparar a predição da sua PINN com a verdade fundamental (ground truth) e calcular o erro real, garantindo que sua implementação está correta.

## Como Implementar com uma PINN (Passo a Passo)

### Arquitetura da Rede

Use uma rede neural simples (MLP - Perceptron de Múltiplas Camadas) que recebe $(x,t)$ como entrada e retorna $\hat{u}(x,t)$ como saída.

* **Entrada:** 2 neurônios (para $x$ e $t$).
* **Camadas Ocultas:** 3 a 4 camadas com 20-30 neurônios cada e funções de ativação como $\tanh$.
* **Saída:** 1 neurônio (para a temperatura $u$).

### Função de Perda (Loss Function)

A perda total é a soma das perdas que forçam a rede a obedecer à física e às condições do problema.

* **Perda da Física ($L_{EDP}$):**
    Defina o resíduo da EDP: 
    
    $$ f = \frac{\partial \hat{u}}{\partial t} - \alpha \frac{\partial^2 \hat{u}}{\partial x^2} $$
    
    Para calcular as derivadas $\frac{\partial \hat{u}}{\partial t}$ e $\frac{\partial^2 \hat{u}}{\partial x^2}$, você usará a diferenciação automática (disponível em frameworks como PyTorch e TensorFlow), aplicando-a diretamente na saída da rede neural $\hat{u}$.

    A perda é o erro quadrático médio do resíduo em um conjunto de pontos aleatórios (collocation points) dentro do domínio ($x,t$):
    $$ L_{EDP} = \frac{1}{N_f} \sum_{i=1}^{N_f} |f(x_i, t_i)|^2 $$
    
* **Perda das Condições de Contorno ($L_{CC}$):**
    Calcule o erro quadrático médio da predição da rede nas fronteiras $x=0$ e $x=L$.
    
    $$ L_{CC} = \frac{1}{N_{cc}} \sum_{i=1}^{N_{cc}} \left( |\hat{u}(0, t_i)|^2 + |\hat{u}(L, t_i)|^2 \right) $$
    
* **Perda da Condição Inicial ($L_{CI}$):**
    Calcule o erro quadrático médio da predição da rede no tempo inicial $t=0$ em relação à condição inicial conhecida (ex: $\sin(\pi x)$).

    $$ L_{CI} = \frac{1}{N_{ci}} \sum_{i=1}^{N_{ci}} |\hat{u}(x_i, 0) - \sin(\pi x_i)|^2 $$

### Perda Total

$$ L_{\text{Total}} = L_{EDP} + L_{CC} + L_{CI} $$

(Às vezes, pesos são adicionados: $L_{\text{Total}} = w_1 L_{EDP} + w_2 L_{CC} + w_3 L_{CI}$).

## Treinamento

* Gere lotes de pontos de colocação para a EDP, as condições de contorno e a condição inicial.
* Use um otimizador (como o Adam) para minimizar a $L_{\text{Total}}$, ajustando os pesos da rede neural.
* Treine por milhares de épocas até que a perda convirja para um valor baixo.
