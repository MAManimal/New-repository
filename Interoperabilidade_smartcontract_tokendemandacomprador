// Contrato do Comprador
contract CompradorContract {
    mapping(address => string) public walletPublic;

    function setWalletPublic(string memory wallet) public {
        walletPublic[msg.sender] = wallet;
    }
}

// Contrato de Geração de Tokens
contract TokenGenerationContract {
    address public owner;
    uint256 public taxa;
    mapping(address => uint256) public tokensGerados;
    mapping(address => uint256) public tempoDeGeracao;

    event TokensGerados(address comprador, uint256 quantidade, uint256 tempoLimite);

    modifier onlyOwner {
        require(msg.sender == owner, "Somente o dono pode chamar essa função");
        _;
    }

    constructor(uint256 _taxa) {
        owner = msg.sender;
        taxa = _taxa;
    }

    function gerarTokens(address comprador, uint256 quantidade, uint256 tempoLimite) public payable onlyOwner {
        require(msg.value >= taxa, "Valor insuficiente para gerar tokens");
        owner.transfer(msg.value);

        tokensGerados[comprador] = quantidade;
        tempoDeGeracao[comprador] = tempoLimite;

        emit TokensGerados(comprador, quantidade, tempoLimite);
    }
}

// Contrato de Interoperabilidade
contract Interoperabilidade {
    CompradorContract public compradorContract;
    TokenGenerationContract public tokenContract;

    constructor(address _compradorContract, address _tokenContract) {
        compradorContract = CompradorContract(_compradorContract);
        tokenContract = TokenGenerationContract(_tokenContract);
    }

    function realizarInteroperabilidade(string memory walletPublic, uint256 tempoLimite, uint256 quantidadeTokens) public {
        compradorContract.setWalletPublic(walletPublic);
        tokenContract.gerarTokens(msg.sender, quantidadeTokens, tempoLimite);
    }
}
