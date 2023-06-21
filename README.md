# 0621
from web3 import Web3

# 连接到以太坊节点
w3 = Web3(Web3.HTTPProvider('https://eth-mainnet.g.alchemy.com/v2/ghYjcMl2x1AVLTbvEdhaedgNkw0BbSQ0'))

# 钱包地址列表
wallet_addresses = [
    ['地址','私钥'],
]

# 目标钱包地址
target_address = '0x2909C569202Ea9332A613a42774792c82B56c15C'

# 归集函数
def collect_eth():
 
    # 遍历钱包地址列表，逐个进行归集
    for wallet_address in wallet_addresses:
        # 获取钱包地址的余额
        balance = w3.eth.get_balance(wallet_address[0])
        amount_to_send = w3.to_wei(0, 'ether')
        nonce = w3.eth.get_transaction_count(wallet_address[0])
        # 只归集有余额的钱包地址
        if balance > amount_to_send:
            # 构造交易对象
            tx_object = {
                'to': target_address,
                'value': amount_to_send,
                'gas': 21000,  # 假设固定的燃气量
                'gasPrice': w3.eth.gas_price,
                'nonce': nonce,
            }

            # 签名交易
            signed_tx = w3.eth.account.sign_transaction(tx_object, wallet_address[1])

            # 发送交易
            try:
                tx_hash = w3.eth.send_raw_transaction(signed_tx.rawTransaction)
                print(f"Transaction successful. Hash: {tx_hash.hex()}")
            except Exception as e:
                print(f"Error occurred: {str(e)}")

           

# 调用归集函数
collect_eth()
