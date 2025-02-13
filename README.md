# B3-Auto-Trader
Este script em Python permite monitorar cotações de ativos da Bolsa de Valores de São Paulo (B3) em tempo real e executar operações de compra e venda com base em regras predefinidas.
import yfinance as yf
import time

# Configurações iniciais
ativos = ["PETR4.SA", "VALE3.SA", "ITUB4.SA"]  # Lista de ativos para monitorar
regras = {
    "PETR4.SA": {"compra": 25.0, "venda": 30.0, "quantidade": 10},
    "VALE3.SA": {"compra": 60.0, "venda": 70.0, "quantidade": 5},
    "ITUB4.SA": {"compra": 20.0, "venda": 25.0, "quantidade": 15}
}

# Função para verificar cotações
def verificar_cotacoes():
    cotacoes = {}
    for ativo in ativos:
        ticker = yf.Ticker(ativo)
        dados = ticker.history(period="1d")
        if not dados.empty:
            preco_atual = dados["Close"].iloc[-1]
            cotacoes[ativo] = preco_atual
    return cotacoes

# Função para verificar regras de operação
def verificar_regras(cotacoes):
    for ativo, preco in cotacoes.items():
        if preco <= regras[ativo]["compra"]:
            print(f"Comprando {regras[ativo]['quantidade']} de {ativo} a {preco}")
            # Aqui iria a ordem de compra na API da corretora
        elif preco >= regras[ativo]["venda"]:
            print(f"Vendendo {regras[ativo]['quantidade']} de {ativo} a {preco}")
            # Aqui iria a ordem de venda na API da corretora

# Função principal
def main():
    print("Monitorando cotações em tempo real... Pressione Ctrl+C para sair.")
    try:
        while True:
            cotacoes = verificar_cotacoes()
            verificar_regras(cotacoes)
            time.sleep(5)  # Atualiza a cada 5 segundos
    except KeyboardInterrupt:
        print("Monitoramento encerrado.")

if __name__ == "__main__":
    main()
