
![](https://i.imgur.com/G5rfCLY.png)

![](https://i.imgur.com/3VX3jXS.png)

![](https://i.imgur.com/dEjXrbs.png)

![](https://i.imgur.com/9qzhUtB.png)

![](https://i.imgur.com/rmXGcNN.png)

![](https://i.imgur.com/7yM6zCB.png)

![](https://i.imgur.com/BrIdUsu.png)

![](https://i.imgur.com/dGDoDXP.png)
CASE 1 에서 부분 또는 하나의 데이터셋 $E_{n}$에 대해서 $W_{kj}$를 구하는 과정

다음 페이지랑 헷갈리지 않기 위해서, 페이지의 $\partial{E}$  가 다음페이지의 $\partial{E_n}$ ($\partial{E} = \partial{E_n}$)
 
![](https://i.imgur.com/RThcGka.png)
이페이지의 $\partial{E}$ 는 전체 데이터셋에 대해서 미분하는 거기 때문에 $\sum$ 합산(기울기 계산(1)참고)

![](https://i.imgur.com/LsJQScQ.png)

![](https://i.imgur.com/mZS8L1g.png)
CASE 2의 경우는 히든레이어의 $net_{nj}$을 구하기 위해서는 모든 $W_{k}$ 의 값이 필요하고 그렇기 위해서 아래 식과 같이 모든 미분값의 합이 필요
$$
\sum_{k=1}^{m} 
\frac{\partial E_n}{\partial o_{nk}} \cdot
\frac{\partial o_{nk}}{\partial net_{nk}} \cdot
\frac{\partial net_{nk}}{\partial h_{nj}}
$$


![](https://i.imgur.com/FXbU4Pm.png)

![](https://i.imgur.com/Ks0xZVp.png)
여기서도 $E_{n}$ 미분 경우 하나 또는 부분 데이터(인풋)에 대한 E값이고
$E$ 는 전체 데이터셋을 의미하기 때문에 전체 $\sum_{n-1}^{N}$ 를 하게된다
