# Memoization: Aumente a Performance das Suas Funções JavaScript

## Introdução

Opa, tudo bem?! Hoje vamos falar sobre algo que vai deixar seu código JavaScript muito mais rápido: memoization! Memoization é uma técnica que armazena o resultado de funções caras (aquelas que demoram pra calcular) para evitar que sejam recalculadas desnecessariamente. Essa ideia bacana surgiu lá atrás, nos estudos de computação para otimizar programas, e hoje está presente em várias linguagens, inclusive no nosso querido JavaScript.

## Onde Utilizar

Agora, você deve estar se perguntando: "Mas onde eu vou usar isso?" A memoization é perfeita para funções que são chamadas várias vezes com os mesmos argumentos, como em cálculos matemáticos pesados, renderização de gráficos e qualquer outra situação onde a performance é crucial. Imagine que você tem uma função que calcula a soma dos primeiros N números. Se você chamar essa função várias vezes com o mesmo N, seria um desperdício recalcular tudo de novo, né? É aí que a memoization entra em ação!

## Benefícios e Pontos de Atenção

Os benefícios são claros: desempenho turbo no seu código, menos cálculos redundantes e mais eficiência! Mas, como nem tudo são flores, é importante ficar de olho no consumo de memória. Guardar muitos resultados pode acabar ocupando bastante espaço, então use com moderação e só quando realmente fizer sentido.

## Armazenamento dos Dados

Os dados são armazenados em um objeto (ou mapa) dentro da função memoizada. Esse objeto serve como um cache, onde cada entrada corresponde a um conjunto específico de argumentos e o resultado correspondente. É como se fosse uma tabela de consulta rápida: você verifica se o resultado já está lá e, se estiver, usa ele direto. Se não, calcula e guarda para a próxima.

## Persistência do Cache no Navegador

Beleza, agora vamos dar um passo além! Até agora, a nossa cache só vive enquanto a página está aberta. Mas e se a gente quiser que essa cache persista mesmo depois de fechar e reabrir o navegador? Para isso, podemos usar o localStorage, que é uma API do navegador que permite armazenar dados de forma persistente. Isso significa que mesmo se o usuário fechar o navegador, os dados ainda estarão lá na próxima vez que ele abrir a página.

## Exemplo Mão na Massa

Vamos botar a mão na massa com um exemplo bem prático! Vamos criar uma função memoizada que calcula o n-ésimo número da sequência de Fibonacci, que é famosa por ser bem pesada pra calcular recursivamente.

```
// Função para criar uma função memoizada com cache armazenado no localStorage
function memoizeWithLocalStorage(fn, key) {
    // Retorna uma nova função que realiza a memoization
    return function (...args) {
        // Gera uma chave única para os argumentos passados para a função
        const cacheKey = JSON.stringify(args);
        // Tenta recuperar o cache do localStorage usando a chave fornecida
        let cache = JSON.parse(localStorage.getItem(key)) || {};

        // Verifica se o resultado para esses argumentos já está no cache
        if (cache[cacheKey]) {
            // Se estiver no cache, retorna o resultado diretamente
            return cache[cacheKey];
        }

        // Se não estiver no cache, calcula o resultado chamando a função original
        const result = fn(...args);
        // Armazena o resultado no cache com a chave gerada
        cache[cacheKey] = result;

        // Atualiza o cache no localStorage com o novo resultado
        localStorage.setItem(key, JSON.stringify(cache));
        // Retorna o resultado calculado
        return result;
    };
}

// Função para calcular o n-ésimo número da sequência de Fibonacci
const fibonacci = memoizeWithLocalStorage(function(n) {
    if (n <= 1) return n;
    // Chama recursivamente a função memoizada para calcular o valor
    return fibonacci(n - 1) + fibonacci(n - 2);
}, 'fibonacciCache');

// Testa a função memoizada com o valor 10
console.log(fibonacci(10));  // Resultado esperado: 55
// Testa novamente para verificar se o cache está funcionando
console.log(fibonacci(10));  // Usando o cache persistente, resultado: 55
```

Nesse exemplo, a função memoizeWithLocalStorage recebe uma função e uma chave para armazenar os dados no localStorage. Na primeira vez que a função é chamada, ela armazena o resultado no localStorage. Nas próximas vezes, se os mesmos argumentos forem passados, ela vai buscar o resultado no localStorage, garantindo que a cache persista mesmo após fechar e reabrir o navegador.

## Conclusão

E aí, gostou dessa técnica? Agora seu cache está ainda mais robusto e persistente, otimizando o desempenho da sua aplicação mesmo entre sessões do navegador.

Esse artigo foi criado por uma IA 🤖 e adaptado e revisado por um humano 🙋‍♂️. Para mais dicas e conteúdos sobre desenvolvimento, fique de olho em meus próximos artigos aqui na DIO!

Até a próxima, e continue explorando o mundo da programação! 🚀

NOTA: Este artigo foi desenvolvido como parte da trilha de Fundamentos de IA para Devs do Santander Bootcamp 2024.