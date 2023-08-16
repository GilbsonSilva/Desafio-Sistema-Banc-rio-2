import textwrap


def menu():
    menu = """\n
    ================ MENU ================
    [d]\tDepositar
    [s]\tSacar
    [e]\tExtrato
    [nc]\tNova conta
    [lc]\tListar contas
    [nu]\tNovo usuário
    [q]\tSair
    => """
    return input(textwrap.dedent(menu))

   
def depositar(saldo, valor, extrato, /, *, numero_de_depositos, LIMITE_De_DEPOSITOS, limite_deposito_dia):
    excedeu_limite_deposito = valor > limite_deposito_dia
    excedeu_numero_deposito = numero_de_depositos >= LIMITE_DE_DEPOSITOS

    if excedeu_limite_deposito:
        print(
            f"Operação falhou! O valor do deposito excede o limite: R${limite_deposito_dia}")
    elif excedeu_numero_deposito:
        print("Operação falhou! Número máximo de depositos excedido.")
        print(f"Numero de operações realizada: {numero_de_depositos}")
        print(F"Limite de Depositos: {LIMITE_DE_DEPOSITOS}")

    elif valor > 0:
        saldo += valor
        numero_de_depositos += 1
        extrato += f"Depósito:\tR$ {valor:.2f}\n"
        print(f"Valor depositado com sucesso: R${valor: .2f}")
        print(f"Numero de operações realizada: {numero_de_depositos}")
    else:
        print("Falha na Operação! O valor informado é inválido insira um valor valido.")

    return saldo, extrato


def sacar(*, saldo, valor, extrato, limite_saque_dia, numero_de_saques, LIMITE_DE_SAQUES):
    excedeu_saldo = valor > saldo
    excedeu_limite = valor > limite_saque_dia
    excedeu_saques = numero_saques >= LIMITE_DE_SAQUES

    if excedeu_saldo:
        print("Operação falhou! Saldo insuficiente.")
        print(f"Saldo atual: R${saldo}")

    elif excedeu_limite:
        print(f"Operação falhou! O valor do saque excede o limite: R${limite_saque_dia}")

    elif excedeu_saques:
        print("Operação falhou! Número máximo de saques excedido.")
        print(f"Numero de operações realizada: {numero_de_saques}")
        print(f"limite de saques: {LIMITE_DE_SAQUES}")

    elif valor > 0:
        saldo -= valor
        extrato += f"nSaque:\t\tR$ {valor:.2f}\n"
        numero_de_saques += 1
        print(f"Saque realizado com sucesso: R${valor: .2f}")
        print(f"Numero de operações realizada: {numero_de_saques}")

    else:
        print("Operação falhou! O valor informado é inválido.")

    return saldo, extrato, numero_de_saques


def exibir_extrato(saldo, /, *, extrato):
    print("\n================ EXTRATO ================")
    print("Extrato".center(36, "="))
    print("Não foram realizadas movimentações." if not extrato else extrato)
    print("".center(36, "="))
    print(f"\nSaldo:\t\tR$ {saldo:.2f}")
    print("==========================================")


def criar_usuario(usuarios):
    cpf = input("Informe o CPF (somente número): ")
    usuario = filtrar_usuario(cpf, usuarios)

    if usuario:
        print("\n====Já existe usuário com esse CPF! ====")
        return

    nome = input("Informe o nome completo: ")
    data_nascimento = input("Informe a data de nascimento (dd-mm-aaaa): ")
    endereco = input("Informe o endereço (logradouro, nro - bairro - cidade/sigla estado): ")

    usuarios.append({"nome": nome, "data_nascimento": data_nascimento, "cpf": cpf, "endereco": endereco})

    print("=== Usuário criado com sucesso! ===")


def filtrar_usuario(cpf, usuarios):
    usuarios_filtrados = [
        usuario for usuario in usuarios if usuario["cpf"] == cpf]
    return usuarios_filtrados[0] if usuarios_filtrados else None


def criar_conta(agencia, numero_conta, usuarios):
    cpf = input("Informe o CPF do usuário: ")
    usuario = filtrar_usuario(cpf, usuarios)

    if usuario:
        print("\n=== Conta criada com sucesso! ===")
        return {"agencia": agencia, "numero_conta": numero_conta, "usuario": usuario}

    print("\n ====Usuário não encontrado, fluxo de criação de conta encerrado! ====")


def listar_contas(contas):
    for conta in contas:
        linha = f"""\
            Agência:\t{conta['agencia']}
            C/C:\t\t{conta['numero_conta']}
            Titular:\t{conta['usuario']['nome']}
        """
        print("=" * 100)
        print(textwrap.dedent(linha))


def main():
    limite_saque_dia = 500
    numero_saques = 0
    LIMITE_DE_SAQUES = 3

    limite_deposito_dia = 1500
    numero_de_depositos = 0
    LIMITE_DE_DEPOSITOS = 5
   
    AGENCIA = "0001"

    saldo = 0
    extrato = ""
   
    usuarios = []
    contas = []

    while True:
        opcao = menu()

        if opcao == "d":
            print(" Deposito".center(40, "="))
            valor = float(input("Informe o valor do depósito: R$"))

            saldo, extrato = depositar(saldo, valor, extrato, numero_de_depositos= numero_de_depositos, LIMITE_DE_DEPOSITOS= LIMITE_DE_DEPOSITOS, limite_deposito_dia= limite_deposito_dia
            )

        elif opcao == "s":
            print("Sacar".center(56, "="))
            valor = float(input("Informe o valor do saque: R$"))

            saldo, extrato = sacar(
                saldo=saldo,
                valor=valor,
                extrato=extrato,
                limite_saque_dia=limite_saque_dia,
                numero_de_saques=numero_de_saques,
                LIMITE_DE_SAQUES==LIMITE_SAQUES,
            )

        elif opcao == "e":
            exibir_extrato(saldo, extrato=extrato)

        elif opcao == "nu":
            criar_usuario(usuarios)

        elif opcao == "nc":
            numero_conta = len(contas) + 1
            conta = criar_conta(AGENCIA, numero_conta, usuarios)

            if conta:
                contas.append(conta)

        elif opcao == "lc":
            listar_contas(contas)

        elif opcao == "f":
            break

        else:
            print("Operação inválida, por favor selecione novamente a operação desejada.")


main()
