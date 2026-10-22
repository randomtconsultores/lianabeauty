# Liana Beauty — redesign do frontend OpenCart 4.1

## Âmbito e estado

A raiz deste repositório corresponde a catalog/view. Não contém uma instalação completa do OpenCart, controllers, modelos, base de dados ou configuração comercial dos módulos.

Branch: feat/lianabeauty-frontend-redesign, criada a partir de main em 4aca7eacd6082924f36ae5392437162d4372aa3c. Alterações publicadas sem modificar main ou reescrever o histórico:

- b2718e99c1b99df430cdea8022150bedeb89910a: ilustração editorial local.
- 0d6dfee9bf56f1ec9618b323d61c5736f8afad9d: sistema visual CSS.
- a84b9bb84fb8315a81c6dfa86a76ac4311b0afdc: integração dos seis templates.

O snapshot parcial usado na primeira implementação não persistiu entre sessões. Os conteúdos foram recuperados do registo dessa implementação e publicados pelo conector GitHub. Os commits locais anteriormente mencionados não foram enviados nem fazem parte do histórico remoto.

## Sistema visual

- Fúcsia #c90060; hover #a6004f; blush #f9e8ee; off-white #fffaf8; ameixa #281822; texto secundário #6e5b65.
- UI com system-ui; títulos editoriais com Georgia. Sem dependências novas de fontes remotas.
- Variáveis CSS, espaçamento com clamp, raios consistentes e controlos principais com mínimo de 44px.
- Escopo body.lb-store; folha adicional carregada depois das folhas declaradas pelos módulos. Não altera bootstrap.css, stylesheet.css ou os fontes Sass existentes.
- Foco visível, skip link, labels acessíveis, tratamento de nomes longos e prefers-reduced-motion. Um único !important efetivo contraria a regra upstream que remove o contorno de foco dos botões.

## Ficheiros e funções

- template/common/header.twig: ativa a skin, reorganiza as colunas, mantém logo configurado e usa marca tipográfica quando logo está vazio. Mantém pesquisa, mini-carrinho e links de conta.
- template/common/home.twig: acrescenta hero editorial sem substituir content_top, content_bottom, column_left ou column_right.
- template/common/menu.twig: conserva categorias, subcategorias, URLs e contratos Bootstrap; acrescenta acessibilidade e classe de mega menu.
- template/common/search.twig: conserva action, POST, search e respetivo valor; acrescenta nomes acessíveis.
- template/common/footer.twig: acrescenta assinatura visual e conserva links condicionais, newsletter, cookie e scripts.
- template/product/thumb.twig: conserva preço, special, tax, rating, compra, favoritos, comparação, mínimo e product_id. Acrescenta lazy loading e nomes acessíveis.
- stylesheet/lianabeauty.css: skin partilhada de header, homepage, produtos, list/grid, navegação, formulários, breadcrumbs, paginação, mensagens, modais, mini-carrinho e footer.
- image/lianabeauty/hero-perfume.svg: ilustração original temporária, sem marca de perfume, scripts ou referências externas. Não representa um produto comercial nem substitui fotografia licenciada de campanha.

## Contratos e dependências

A validação estática inicial comparou os seis templates com os originais e confirmou a preservação dos href, action, formaction, métodos, nomes e valores de campos, IDs e atributos data-oc-* e data-bs-* originais, por multiconjunto. As classes visuais foram ajustadas intencionalmente; não se afirma que todas as classes sejam idênticas.

Não foram alterados controllers, modelos, lógica de negócio, common.js ou bibliotecas. O mini-carrinho conserva #cart e text_items: não se extrai nem inventa uma quantidade. Nenhum formulário de newsletter foi acrescentado; conserva-se o link funcional existente.

jQuery 3.7.1 está identificado pelo nome do ficheiro existente. Bootstrap e Magnific Popup estão presentes; as versões exatas dos bundles servidos pela instalação não foram confirmadas. Nenhuma dependência foi atualizada ou duplicada.

Os estilos de .carousel conservam a inicialização, os itens e os indicadores dos módulos. Não há nova inicialização JavaScript. A existência de outras extensões de carrossel não foi verificada.

## Limitações conhecidas

1. Os templates das extensões de módulos não estão na árvore inspecionada. content_top e content_bottom recebem HTML já renderizado; não foi possível determinar os módulos ativos, os produtos disponibilizados ou a ordem comercial configurada.
2. O CTA do hero aponta para a secção existente #lb-discover. Não aponta ainda para uma rota de categoria/campanha: essa ligação exige uma URL gerada pelo controller ou um banner configurável. Não foram inventadas categorias ou URLs. Se os módulos estiverem vazios, configurar produtos antes de apresentar a homepage comercialmente.
3. A copy editorial nova está em PT-PT nos templates. Para outras línguas, deve ser integrada nos ficheiros de idioma e controllers correspondentes.
4. Substituir o SVG temporário por fotografia licenciada de campanha. O logótipo configurado continua a prevalecer; substituir qualquer logo demo na administração.
5. Categoria, produto, pesquisa, carrinho e conta recebem estilos partilhados, mas os seus templates não foram reimplementados.
6. Não foram adicionadas avaliações, descontos, certificações, promessas de entrega ou dados de demonstração aos templates.

## Verificações executadas

Na primeira implementação local, antes da publicação:

- SHA-1 Git dos seis blobs originais: coincidente com as leituras do GitHub.
- Comparação dos atributos funcionais dos seis templates: passou.
- Parser CSS tinycss2, incluindo regras e declarações em media queries: sem erros encontrados na análise executada.
- SVG: XML válido, sem scripts ou referências externas.
- Recursos novos: existência confirmada no snapshot; common.js original confirmado no repositório.
- Smoke parse de seis templates com Jinja2, adaptando o range Twig 1..5. NÃO equivale a compilar com o motor PHP/Twig.
- Treze renders sintéticos passaram: cartões com/sem special e rating 0/3/5; menus com 0/1/5/21 subcategorias; header anónimo/autenticado; inserção única dos módulos na homepage. Fixtures usadas apenas nos testes, não na loja.
- Contraste calculado dos tokens: branco/fúcsia 5.75:1; ameixa/blush 14.31:1; texto secundário/off-white 6.06:1. Não constitui auditoria da página renderizada.
- git diff --check: passou no snapshot local.
- JavaScript: nenhum ficheiro acrescentado ou alterado; não foi executado teste de sintaxe JS.

Após a publicação, o conector confirmou os SHAs dos três commits e a leitura do commit a84b9bb confirmou os seis templates modificados. Os testes locais não foram novamente executados sobre um checkout completo da branch remota.

NÃO executado: compilação PHP/Twig real; instalação OpenCart e base de dados; browser ou screenshots; viewports; AJAX; compra com opções/subscrições; mini-carrinho; checkout; autenticação; moeda/idioma; cookies e carrosséis em runtime. Não se afirma que estes fluxos tenham sido testados funcionalmente.

## Testar em staging

1. Num clone real do repositório: git fetch origin e git switch --track origin/feat/lianabeauty-frontend-redesign, caso a branch local ainda não exista. Se já existir, selecionar a branch e rever o estado antes de atualizar.
2. Rever o diff relativamente a main e fazer backup da instalação. Publicar apenas os seis templates modificados, stylesheet/lianabeauty.css e image/lianabeauty/hero-perfume.svg nas posições correspondentes em catalog/view. Não apagar nem espelhar os restantes ficheiros da instalação.
3. Atualizar a cache de tema/Twig pelo mecanismo disponível na instalação e forçar a atualização do browser. Não substituir configurações privadas.
4. Confirmar respostas HTTP 200 para o CSS e SVG e ausência de erros de consola/network. Compilar os templates com o Twig realmente instalado.
5. Verificar 320, 375, 768, 1024 e 1440px, nomes longos, menus extensos, zoom 200%, teclado e foco. Verificar ausência de overflow horizontal involuntário.
6. Testar pesquisa, ordenação, limite, list/grid, produto simples e com opções, mínimo maior que 1, favoritos, comparação, adicionar/remover/alterar quantidades no carrinho e checkout.
7. Testar registo/login/logout, mensagens de validação, moeda/idioma, modais e consentimento de cookies.
8. Testar cada carrossel real com zero, um, dois e muitos itens; navegação, indicadores, mobile e reduced-motion.
9. Rever logótipo, imagem de campanha e módulos comerciais antes de produção.

Rollback: reverter os commits de implementação após revisão, ou repor os seis templates originais e retirar os dois recursos novos, atualizando a cache. Não remover ficheiros de módulos nem reescrever o histórico.
