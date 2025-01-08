Aqui está o conteúdo completo em formato markdown, sem renderização, pronto para ser copiado diretamente para o seu arquivo `README.md`:

```
# K3L Collectible

**K3L Collectible** é um contrato ERC721 avançado que permite a criação, gestão e uso de NFTs com diversas funcionalidades adicionais, incluindo raridade, staking, whitelist para presale, e mais.

---

## 📋 Características

### Informações Básicas
- **Nome:** K3L Collectible  
- **Símbolo:** K3L  
- **Padrão:** ERC721  

### Recursos Avançados
- ✅ **Whitelist:** Controle de endereços autorizados para presale  
- 🎯 **Staking:** Possibilidade de "travar" NFTs para benefícios futuros  
- 🔑 **Presale:** Configuração de preços diferenciados para pré-venda  
- 🔥 **Queima:** Permite a destruição de tokens  
- 📈 **Raridade:** Diferentes níveis de raridade para os NFTs  
- 🛑 **Pausável:** Transferências e ações podem ser pausadas em emergências  
- 🛒 **Taxa de Marketplace:** Suporte para cálculo automático de taxas em vendas  

---

## 🚀 Instalação

1. Clone o repositório do contrato ou copie o código.
2. Certifique-se de ter os pacotes do **OpenZeppelin** instalados:
   ```bash
   npm install @openzeppelin/contracts
   ```
3. Compile e implemente o contrato usando Hardhat ou Truffle.

---

## 📚 Uso

### Implantação
Implemente o contrato fornecendo o endereço do tesouro:
```javascript
const K3LCollectible = await ethers.getContractFactory("K3LCollectible");
const k3lCollectible = await K3LCollectible.deploy(treasuryAddress);
await k3lCollectible.deployed();
```

### Funções Principais

#### Mint (Criação de NFTs)
Crie um novo NFT especificando o destinatário, URI do token e raridade:
```javascript
await k3lCollectible.mint(recipient, "ipfs://tokenURI", 2); // 2 representa Epic
```

#### Stake e Unstake
Bloqueie ou desbloqueie NFTs:
```javascript
await k3lCollectible.stakeNFT(tokenId);    // Stake
await k3lCollectible.unstakeNFT(tokenId);  // Unstake
```

#### Queima
Destrua um NFT:
```javascript
await k3lCollectible.burn(tokenId);
```

#### Gerenciar Presale
Ativar/desativar presale:
```javascript
await k3lCollectible.togglePresale();
```

Adicionar/Remover usuários da whitelist:
```javascript
await k3lCollectible.addToWhitelist(userAddress);
await k3lCollectible.removeFromWhitelist(userAddress);
```

#### Taxa de Marketplace
Obtenha a taxa aplicável para vendas:
```javascript
const fee = await k3lCollectible.calculateMarketplaceFee(salePrice);
```

#### Atualizar URI do Token
Atualize o URI de um token (somente pelo owner):
```javascript
await k3lCollectible.updateTokenURI(tokenId, "ipfs://newTokenURI");
```

---

## 🔒 Segurança
- **Controles de Acesso:** Funções administrativas protegidas com `onlyOwner`.
- **Sistema de Pausa:** Para gerenciar emergências.
- **Whitelist:** Somente endereços autorizados podem participar da presale.

---

## ⚠️ Avisos Importantes
- Audite o contrato antes do uso em produção.
- Use `onlyOwner` com responsabilidade.
- Teste extensivamente antes de qualquer deploy na mainnet.
- Gerencie as chaves privadas com segurança.

---

## 📄 Licença
Distribuído sob a licença MIT. Veja `LICENSE.md` para detalhes.

---

## 🤝 Suporte
Para dúvidas e suporte, abra uma issue no repositório do GitHub.
```

Agora você pode copiar e colar todo o conteúdo acima no seu arquivo `README.md` sem problemas.
