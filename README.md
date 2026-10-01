# H-HPU
Plataforma de Aceleração Computacional (PAC)







## 📊 Resultados de Benchmarking e Simulação (Validação v0.4/v0.5)

Avaliação da **Arquitetura H-HPU** em relação a uma referência padrão de malha (Grid) 2D ($N = 1024$ nós) sob cargas de trabalho hierárquicas/relacionais (padrão de tráfego $\mathcal{O}(N \log N)$).

### Principais Métricas de Desempenho

| Métrica | Referência (Malha 2D) | H-HPU (H6-NoC + H-MEM) | Delta / Impacto | Fator Principal |
| :--- | :--- | :--- | :--- | :--- |
| **Diâmetro da Rede** | $62,0$ saltos | $36,0$ saltos | **-41,9%** | Compatibilidade com a equação de topologia (margem de 1,5%) |
| **Largura de Banda de Bisseção** | $31,6$ links | $73,0$ links | **+130,9%** | Conectividade planar hexagonal ($d \le 6$) |
| **Média de Saltos** | $7,70$ saltos | $7,66$ saltos | ~0% | Alta localidade espacial na carga hierárquica |
| **Latência Média** | $21,4$ ciclos | $9,9$ ciclos | **-53,7%** | Redução massiva de contenção nas filas dos roteadores |
| **Vazão de Saturação** | $0,10$ flits/ciclo | $0,143$ flits/ciclo | **+43,0%** | Malha satura precocemente; H6 absorve a injeção |
| **Pressão de Memória ($U_M$)** | $1,034$ (saturado) | $0,932$ (estável) | **-9,8%** | Controle de malha fechada HARC ($a \approx 0,10$) |
| **Sobrecarga de Coerência** | $1026$ msgs/escrita | $3,2$ msgs/escrita | **-99,7%** | Máscara direcional de 6 vizinhos do H-MESI |

### Principais Conclusões Arquiteturais

1. **Vazão em detrimento de saltos:** Em cenários de tráfego localizado, a latência cai 53,7% não porque os pacotes percorrem menos saltos, mas porque a largura de banda de bisseção 2,3 vezes maior evita a saturação dos buffers de fila em taxas de injeção $> 0,10$.
2. **Estabilidade do HARC:** O controlador HARC converge para $U_M \approx 0,932$, mantendo a pressão de memória estritamente abaixo da saturação, sem desencadear condições de corrida de coerência ou *deadlocks*.
3. **Invalidação Direcionada:** O H-MESI restringe o tráfego de invalidação de cache estritamente ao cluster de 6 vizinhos, substituindo efetivamente as difusões globais (*broadcasts*) por atualizações locais direcionadas.
