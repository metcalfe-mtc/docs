* [1 创建账号](#1-%E5%88%9B%E5%BB%BA%E8%B4%A6%E5%8F%B7)
* [2 查询账号信息](#2-%E6%9F%A5%E8%AF%A2%E8%B4%A6%E5%8F%B7%E4%BF%A1%E6%81%AF)
* [3 签名](#3-%E7%AD%BE%E5%90%8D)
* [4\.1 在线支付](#41-%E5%9C%A8%E7%BA%BF%E6%94%AF%E4%BB%98)
* [4\.2 离线签名支付](#42-%E7%A6%BB%E7%BA%BF%E7%AD%BE%E5%90%8D%E6%94%AF%E4%BB%98)
* [5 账户交易历史](#5-%E8%B4%A6%E6%88%B7%E4%BA%A4%E6%98%93%E5%8E%86%E5%8F%B2)
* [6 查询交易详情](#6-%E6%9F%A5%E8%AF%A2%E4%BA%A4%E6%98%93%E8%AF%A6%E6%83%85)
* [7 获取当前已验证区块高度](#7-%E8%8E%B7%E5%8F%96%E5%BD%93%E5%89%8D%E5%B7%B2%E9%AA%8C%E8%AF%81%E5%8C%BA%E5%9D%97%E9%AB%98%E5%BA%A6)

## 1 创建账号

Request:

```
{
    "method": "wallet_propose"
}
```

| 字段名称 | 类型   | 描述         |
| :------- | :----- | :----------- |
| method   | string | RPC 接口名称 |

Response:

```
{
    "result": {
        "account_id": "MTCwAMYfK8hrvupJWbyriut2Uy2qUKmYsyjy",
        "key_type": "secp256k1",
        "master_key": "RUM RUSH GUT AMOS AVON REIN WOOL TREK GURU HORN CHEW LINT",
        "master_seed": "shsw6nHv35weDpR7i4BBoF8nH8Mzp",
        "master_seed_hex": "605D66CD45EE7DFD9CFD44A65F947A39",
        "public_key": "aBQ1LWN428rKPkW88ViUgfr6zn25VboPAyN93Z2mZShWgr32sDsE",
        "public_key_hex": "037F5D059847FB35D3199AF634F1FDE3CB1BF706D4F77FE85131D294CA034E1A69",
        "status": "success"
    }
}
```

| 字段名称        | 类型   | 描述               |
| :-------------- | :----- | :----------------- |
| account_id      | string | 账号地址           |
| key_type        | string | 生成账号的加密算法 |
| master_key      | string | 助记词             |
| master_seed     | string | 私钥               |
| master_seed_hex | string | Hex 编码私钥       |
| publick_key     | string | 公钥               |
| publick_key_hex | string | Hex 编码公钥       |
| status          | string | 响应状态           |

## 2 查询账号信息

Request:

```
{
    "method": "account_info",
    "params": [
        {
            "account": "MTCwjBNR5YBoQQMign51ED59sTkx1DhtiiTu"
            "ledger_index": "current"
        }
    ]
}
```

| 字段名称     | 类型                 | 描述                                                            |
| :----------- | :------------------- | :-------------------------------------------------------------- |
| account      | string               | 账号                                                            |
| ledger_index | string、unsigned int | 账本高度                                                        |


Response:

```
{
    "result": {
        "account_data": {
            "Account": "MTCwjBNR5YBoQQMign51ED59sTkx1DhtiiTu",
            "Balance": "9999074196072",
            "Flags": 0,
            "LedgerEntryType": "AccountRoot",
            "OwnerCount": 2,
            "PreviousTxnID": "684CC1262FA186EF34E7992D220671F23BA4DDD6D56213EE07197357027431E2",
            "PreviousTxnLgrSeq": 10206,
            "Sequence": 12,
            "index": "4D616F2243FBBD36E1C768F6124C311205844E60EE702CE3F033B2E63119FD7E"
        },
        "ledger_current_index": 17435,
        "status": "success",
        "validated": false
    }
}
```

| 字段名称          | 类型         | 描述                                                         |
| :---------------- | :----------- | :----------------------------------------------------------- |
| Account           | string       | 账号地址                                                     |
| Balance           | string       | 账号 M 余额，单位为Drops。**1 M=1000000 Drops**，即一百万Drops |
| Flags             | unsigned int | 标志                                                         |
| PreviousTxnID     | string       | 上一次交易的 HashID                                          |
| PreviousTxnLgrSeq | unsigned int | 上一次交易的账本高度                                         |
| Sequence          | unsigned int | 交易流水号                                                   |
| index             | string       | 账号对象的 HashID                                            |
| status            | string       | 响应状态                                                     |

## 3 签名

Request:

```
{
    "method": "sign",
    "params": [
        {
            "offline": false,
            "secret": "sh***************************nq",
            "tx_json": {
                "Account": "MTCwjBNR5YBoQQMign51ED59sTkx1DhtiiTu",
                "Amount": 100,
                "Destination": "MTCGqLp9dri37aME12EMa4qrAMDoPdFjaW8M",
                "TransactionType": "Payment"
            }
        }
    ]
}


```

| 字段名称        | 类型                 | 描述                      |
| :-------------- | :------------------- | :------------------------ |
| offline         | bool                 | 如果是 true，表示离线签名 |
| secret          | string               | 私钥                      |
| tx_json         | object               | 交易要素                  |
| Account         | string               | 源账号地址                |
| Amount          | unsigned int、object | 交易额                    |
| Destination     | string               | 目标账号地址              |
| TransactionType | string               | 交易类型                  |

Response:

```
{
    "result": {
        "status": "success",
        "tx_blob": "1200002280000000240000000C61400000000000006468400000000000006473210354F197E0FB1CE445F532DDB83339F20CADE87A21ACF45E9C5A79697CC2E16A9E74473045022100E1B350782C8ABEAED12D3709B6C6289B16AF4166E9668CE5DF134E052A03302402201813C2D4979D3E0A6BA2AD6F7F994BDFE4691D2647CBB08B222C94EF454720A081146AB324ACE019B4EDC90DD29FFF261D44FC445D678314ADADBAD7E728C0C7E693D9D736025BFBF44E80DF",
        "tx_json": {
            "Account": "MTCwjBNR5YBoQQMign51ED59sTkx1DhtiiTu",
            "Amount": "100",
            "Destination": "MTCGqLp9dri37aME12EMa4qrAMDoPdFjaW8M",
            "Fee": "100",
            "Flags": 2147483648,
            "Sequence": 12,
            "SigningPubKey": "0354F197E0FB1CE445F532DDB83339F20CADE87A21ACF45E9C5A79697CC2E16A9E",
            "TransactionType": "Payment",
            "TxnSignature": "3045022100E1B350782C8ABEAED12D3709B6C6289B16AF4166E9668CE5DF134E052A03302402201813C2D4979D3E0A6BA2AD6F7F994BDFE4691D2647CBB08B222C94EF454720A0",
            "hash": "B8AB124DB3214D95D34C771A626F3890E2099F53E43CC1B49BDF2C1D6A541100"
        }
    }
}
```

| 字段名称      | 类型   | 描述                     |
| :------------ | :----- | :----------------------- |
| tx_blob       | string | 签名结果，二进制数据格式 |
| tx_json       | object | 签名要素内容，json 格式  |
| SigningPubKey | string | 签名的公钥               |
| TxnSignature  | string | 交易签名摘要             |
| status        | string | 响应状态                 |

## 4.1 在线支付

Request:

```
{
    "method": "submit",
    "params": [
        {
            "offline": false,
            "secret": "sh***********************nq",
            "tx_json": {
                "Account": "MTCwjBNR5YBoQQMign51ED59sTkx1DhtiiTu",
                "Amount": 100,
                "Destination": "MTCGqLp9dri37aME12EMa4qrAMDoPdFjaW8M",
                "TransactionType": "Payment",
                "Memos": [
                    {
                        "Memo": {
                            "MemoData": "74657374",
                            "MemoType": "737472696E67"
                        }
                    }
                ]
            }
        }
    ]
}
```

| 字段名称 | 类型                | 描述                                                                    |
| :------- | :------------------ | :---------------------------------------------------------------------- |
| method   | string              | RPC 接口名称                                                            |
| offline  | bool                | 是否离线提交。默认 false                                                |
| Amount   | unsignedint，object | 金额                                                                    |
| Memos    | object\[\]          | （可选）备注集                                                          |
| Memo     | object              | 备注                                                                    |
| MemoData | string              | 备注内容，必须是 Hex 编码格式。例如“test”，Hex 编码后是“74657374”       |
| MemoType | string              | 备注类型，必须是 Hex 编码格式。例如“string”，Hex 编码后是“737472696E67” |

Response:

```
{
    "result": {
        "engine_result": "tesSUCCESS",
        "engine_result_code": 0,
        "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
        "status": "success",
        "tx_blob": "1200002280000000240000000D61400000000000006468400000000000006473210354F197E0FB1CE445F532DDB83339F20CADE87A21ACF45E9C5A79697CC2E16A9E74473045022100CA005A0443658B9D600E92879D0545AC0E9D52D2F30C2A2FEAE2315B1204F0ED02203DC9FE07D649AAC6DAC7BD1C8281243EFA15A8F333E58C1A57AB6C5E1C18413181146AB324ACE019B4EDC90DD29FFF261D44FC445D678314ADADBAD7E728C0C7E693D9D736025BFBF44E80DFF9EA7C06737472696E677D0474657374E1F1",
        "tx_json": {
            "Account": "MTCwjBNR5YBoQQMign51ED59sTkx1DhtiiTu",
            "Amount": "100",
            "Destination": "MTCGqLp9dri37aME12EMa4qrAMDoPdFjaW8M",
            "Fee": "100",
            "Flags": 2147483648,
            "Memos": [
                {
                    "Memo": {
                        "MemoData": "74657374",
                        "MemoType": "737472696E67"
                    }
                }
            ],
            "Sequence": 13,
            "SigningPubKey": "0354F197E0FB1CE445F532DDB83339F20CADE87A21ACF45E9C5A79697CC2E16A9E",
            "TransactionType": "Payment",
            "TxnSignature": "3045022100CA005A0443658B9D600E92879D0545AC0E9D52D2F30C2A2FEAE2315B1204F0ED02203DC9FE07D649AAC6DAC7BD1C8281243EFA15A8F333E58C1A57AB6C5E1C184131",
            "hash": "221E1F3313A13281CC49B8F5ACF0EEF57A3B258CFEF3FCB1465007FA8228DBA1"
        }
    }
}
```

| 字段名称              | 类型       | 描述                                                                    |
| :-------------------- | :--------- | :---------------------------------------------------------------------- |
| engine_result         | string     | 支付执行结果                                                            |
| engine_result_code    | int        | 支付结果代码                                                            |
| engine_result_message | string     | 支付结果信息                                                            |
| tx_blob               | string     | 签名结果，二进制数据格式                                                |
| tx_json               | object     | 交易要素，json 格式                                                     |
| Memos                 | object\[\] | （可选）备注集                                                          |
| Memo                  | object     | 备注                                                                    |
| MemoData              | string     | 备注内容，必须是 Hex 编码格式。例如“test”，Hex 编码后是“74657374”       |
| MemoType              | string     | 备注类型，必须是 Hex 编码格式。例如“string”，Hex 编码后是“737472696E67” |
| Fee                   | string     | 交易燃料费，单位为滴                                                    |
| status                | string     | 响应状态                                                                |

## 4.2 离线签名支付

Request:

```
{
    "method": "submit",
    "params": [
        {
            "tx_blob": "1200002280000000240000000D61400000000000006468400000000000006473210354F197E0FB1CE445F532DDB83339F20CADE87A21ACF45E9C5A79697CC2E16A9E74473045022100CA005A0443658B9D600E92879D0545AC0E9D52D2F30C2A2FEAE2315B1204F0ED02203DC9FE07D649AAC6DAC7BD1C8281243EFA15A8F333E58C1A57AB6C5E1C18413181146AB324ACE019B4EDC90DD29FFF261D44FC445D678314ADADBAD7E728C0C7E693D9D736025BFBF44E80DFF9EA7C06737472696E677D0474657374E1F1"
        }
    ]
}
```

| 字段名称 | 类型   | 描述                     |
| :------- | :----- | :----------------------- |
| method   | string | RPC 接口名称             |
| tx_blob  | string | 签名结果，二进制数据格式 |

Response:

```
{
    "result": {
        "engine_result": "tesSUCCESS",
        "engine_result_code": 0,
        "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
        "status": "success",
        "tx_blob": "1200002280000000240000000D61400000000000006468400000000000006473210354F197E0FB1CE445F532DDB83339F20CADE87A21ACF45E9C5A79697CC2E16A9E74473045022100CA005A0443658B9D600E92879D0545AC0E9D52D2F30C2A2FEAE2315B1204F0ED02203DC9FE07D649AAC6DAC7BD1C8281243EFA15A8F333E58C1A57AB6C5E1C18413181146AB324ACE019B4EDC90DD29FFF261D44FC445D678314ADADBAD7E728C0C7E693D9D736025BFBF44E80DFF9EA7C06737472696E677D0474657374E1F1",
        "tx_json": {
            "Account": "MTCwjBNR5YBoQQMign51ED59sTkx1DhtiiTu",
            "Amount": "100",
            "Destination": "MTCGqLp9dri37aME12EMa4qrAMDoPdFjaW8M",
            "Fee": "100",
            "Flags": 2147483648,
            "Memos": [
                {
                    "Memo": {
                        "MemoData": "74657374",
                        "MemoType": "737472696E67"
                    }
                }
            ],
            "Sequence": 13,
            "SigningPubKey": "0354F197E0FB1CE445F532DDB83339F20CADE87A21ACF45E9C5A79697CC2E16A9E",
            "TransactionType": "Payment",
            "TxnSignature": "3045022100CA005A0443658B9D600E92879D0545AC0E9D52D2F30C2A2FEAE2315B1204F0ED02203DC9FE07D649AAC6DAC7BD1C8281243EFA15A8F333E58C1A57AB6C5E1C184131",
            "hash": "221E1F3313A13281CC49B8F5ACF0EEF57A3B258CFEF3FCB1465007FA8228DBA1"
        }
    }
}
```

| 字段名称              | 类型   | 描述                     |
| :-------------------- | :----- | :----------------------- |
| engine_result         | string | 支付执行结果             |
| engine_result_code    | int    | 支付结果代码             |
| engine_result_message | string | 支付结果信息             |
| tx_blob               | string | 签名结果，二进制数据格式 |
| tx_json               | object | 交易要素，json 格式      |
| Fee                   | string | 交易燃料费               |
| status                | string | 响应状态                 |

## 5 账户交易历史

Request:

```
{
    "method": "account_tx",
    "params": [
        {
            "account": "MTCwjBNR5YBoQQMign51ED59sTkx1DhtiiTu",
            "binary": false,
            "forward": false,
            "ledger_index_max": -1,
            "ledger_index_min": -1,
            "limit": 2
        }
    ]
}
```

| 字段名称         | 类型         | 描述                                                                                     |
| :--------------- | :----------- | :--------------------------------------------------------------------------------------- |
| method           | string       | RPC 接口名称                                                                             |
| binnary          | bool         | 返回的交易记录格式是否以二进制格式，（默认）false，即用 json 格式，true 表示用二进制格式 |
| forward          | bool         | 交易记录排序方向                                                                         |
| ledger_index_min | int          | 账本起始高度，-1 表示从最小开始                                                          |
| ledger_index_max | int          | 账本结束高度，-1 表示到当前结束                                                          |
| limit            | unsigned int | 返回记录的分页大小                                                                       |

Response:

```
{
    "result": {
        "account": "MTCwjBNR5YBoQQMign51ED59sTkx1DhtiiTu",
        "ledger_index_max": 17476,
        "ledger_index_min": 1,
        "limit": 2,
        "marker": {
            "ledger": 10206,
            "seq": 0
        },
        "status": "success",
        "transactions": [
            {
                "meta": {
                    "AffectedNodes": [
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "Account": "MTCwjBNR5YBoQQMign51ED59sTkx1DhtiiTu",
                                    "Balance": "9999074195672",
                                    "Flags": 0,
                                    "Issues": [
                                        "757EBEDEF84C121BF42D74673577CD13B0921DA6752732FBF7AD9E01DBA1C3B0",
                                        "1B57F8092C1B718299ED8840D9A825F2E86A6A22B6C0A2E233F04D7E0E9E356C"
                                    ],
                                    "OwnerCount": 2,
                                    "Sequence": 14
                                },
                                "LedgerEntryType": "AccountRoot",
                                "LedgerIndex": "4D616F2243FBBD36E1C768F6124C311205844E60EE702CE3F033B2E63119FD7E",
                                "PreviousFields": {
                                    "Balance": "9999074195872",
                                    "Sequence": 13
                                },
                                "PreviousTxnID": "B8AB124DB3214D95D34C771A626F3890E2099F53E43CC1B49BDF2C1D6A541100",
                                "PreviousTxnLgrSeq": 17466
                            }
                        },
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "Account": "MTCGqLp9dri37aME12EMa4qrAMDoPdFjaW8M",
                                    "Balance": "999990000",
                                    "Flags": 8388608,
                                    "Issues": [
                                        "D5BFA859E9DFA1D66637ADFFB6A1646C4C0F42FFC19FBDBEEAF2BF102E7F02D4"
                                    ],
                                    "OwnerCount": 0,
                                    "Sequence": 4
                                },
                                "LedgerEntryType": "AccountRoot",
                                "LedgerIndex": "AD333501F4523A69A1ED1EEDBA436068718ED8FE230DFD8A1FDF707E7CC12D53",
                                "PreviousFields": {
                                    "Balance": "999989900"
                                },
                                "PreviousTxnID": "B8AB124DB3214D95D34C771A626F3890E2099F53E43CC1B49BDF2C1D6A541100",
                                "PreviousTxnLgrSeq": 17466
                            }
                        },
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "Account": "MTCJNfG2CPaNUx8hgDj5EZMceWUuyVM9ga8B",
                                    "Balance": "72400",
                                    "Flags": 0,
                                    "OwnerCount": 0,
                                    "Sequence": 1
                                },
                                "LedgerEntryType": "AccountRoot",
                                "LedgerIndex": "EDC80B6A0C901F9EA3B1987AAEDED5690185AFA838066DE94A60E842440717EF",
                                "PreviousFields": {
                                    "Balance": "72300"
                                },
                                "PreviousTxnID": "B8AB124DB3214D95D34C771A626F3890E2099F53E43CC1B49BDF2C1D6A541100",
                                "PreviousTxnLgrSeq": 17466
                            }
                        }
                    ],
                    "TransactionIndex": 0,
                    "TransactionResult": "tesSUCCESS",
                    "delivered_amount": "100"
                },
                "tx": {
                    "Account": "MTCwjBNR5YBoQQMign51ED59sTkx1DhtiiTu",
                    "Amount": "100",
                    "Destination": "MTCGqLp9dri37aME12EMa4qrAMDoPdFjaW8M",
                    "Fee": "100",
                    "Flags": 2147483648,
                    "Memos": [
                        {
                            "Memo": {
                                "MemoData": "74657374",
                                "MemoType": "737472696E67"
                            }
                        }
                    ],
                    "Sequence": 13,
                    "SigningPubKey": "0354F197E0FB1CE445F532DDB83339F20CADE87A21ACF45E9C5A79697CC2E16A9E",
                    "TransactionType": "Payment",
                    "TxnSignature": "3045022100CA005A0443658B9D600E92879D0545AC0E9D52D2F30C2A2FEAE2315B1204F0ED02203DC9FE07D649AAC6DAC7BD1C8281243EFA15A8F333E58C1A57AB6C5E1C184131",
                    "date": 654836470,
                    "hash": "221E1F3313A13281CC49B8F5ACF0EEF57A3B258CFEF3FCB1465007FA8228DBA1",
                    "inLedger": 17470,
                    "ledger_index": 17470
                },
                "validated": true
            },
            {
                "meta": {
                    "AffectedNodes": [
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "Account": "MTCwjBNR5YBoQQMign51ED59sTkx1DhtiiTu",
                                    "Balance": "9999074195872",
                                    "Flags": 0,
                                    "Issues": [
                                        "757EBEDEF84C121BF42D74673577CD13B0921DA6752732FBF7AD9E01DBA1C3B0",
                                        "1B57F8092C1B718299ED8840D9A825F2E86A6A22B6C0A2E233F04D7E0E9E356C"
                                    ],
                                    "OwnerCount": 2,
                                    "Sequence": 13
                                },
                                "LedgerEntryType": "AccountRoot",
                                "LedgerIndex": "4D616F2243FBBD36E1C768F6124C311205844E60EE702CE3F033B2E63119FD7E",
                                "PreviousFields": {
                                    "Balance": "9999074196072",
                                    "Sequence": 12
                                },
                                "PreviousTxnID": "684CC1262FA186EF34E7992D220671F23BA4DDD6D56213EE07197357027431E2",
                                "PreviousTxnLgrSeq": 10206
                            }
                        },
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "Account": "MTCGqLp9dri37aME12EMa4qrAMDoPdFjaW8M",
                                    "Balance": "999989900",
                                    "Flags": 8388608,
                                    "Issues": [
                                        "D5BFA859E9DFA1D66637ADFFB6A1646C4C0F42FFC19FBDBEEAF2BF102E7F02D4"
                                    ],
                                    "OwnerCount": 0,
                                    "Sequence": 4
                                },
                                "LedgerEntryType": "AccountRoot",
                                "LedgerIndex": "AD333501F4523A69A1ED1EEDBA436068718ED8FE230DFD8A1FDF707E7CC12D53",
                                "PreviousFields": {
                                    "Balance": "999989800"
                                },
                                "PreviousTxnID": "8972363F01CC7B2398405B8A0799622B92AFEEDD98C67542773014D4E8A35BE6",
                                "PreviousTxnLgrSeq": 491
                            }
                        },
                        {
                            "ModifiedNode": {
                                "FinalFields": {
                                    "Account": "MTCJNfG2CPaNUx8hgDj5EZMceWUuyVM9ga8B",
                                    "Balance": "72300",
                                    "Flags": 0,
                                    "OwnerCount": 0,
                                    "Sequence": 1
                                },
                                "LedgerEntryType": "AccountRoot",
                                "LedgerIndex": "EDC80B6A0C901F9EA3B1987AAEDED5690185AFA838066DE94A60E842440717EF",
                                "PreviousFields": {
                                    "Balance": "72200"
                                },
                                "PreviousTxnID": "844C8596B35E55C0909C0020FA04F90D75A8D2D20F1DC7AEAE6D52DEF780BF66",
                                "PreviousTxnLgrSeq": 10227
                            }
                        }
                    ],
                    "TransactionIndex": 0,
                    "TransactionResult": "tesSUCCESS",
                    "delivered_amount": "100"
                },
                "tx": {
                    "Account": "MTCwjBNR5YBoQQMign51ED59sTkx1DhtiiTu",
                    "Amount": "100",
                    "Destination": "MTCGqLp9dri37aME12EMa4qrAMDoPdFjaW8M",
                    "Fee": "100",
                    "Flags": 2147483648,
                    "Sequence": 12,
                    "SigningPubKey": "0354F197E0FB1CE445F532DDB83339F20CADE87A21ACF45E9C5A79697CC2E16A9E",
                    "TransactionType": "Payment",
                    "TxnSignature": "3045022100E1B350782C8ABEAED12D3709B6C6289B16AF4166E9668CE5DF134E052A03302402201813C2D4979D3E0A6BA2AD6F7F994BDFE4691D2647CBB08B222C94EF454720A0",
                    "date": 654836430,
                    "hash": "B8AB124DB3214D95D34C771A626F3890E2099F53E43CC1B49BDF2C1D6A541100",
                    "inLedger": 17466,
                    "ledger_index": 17466
                },
                "validated": true
            }
        ]
    }
}
```

| 字段名称          | 类型         | 描述             |
| :---------------- | :----------- | :--------------- |
| marker            | object       | 分页标志         |
| meta              | object       | 交易记录原始信息 |
| AffectedNodes     | object       | 影响的单元       |
| ModifiedNode      | object       | 修改的单元       |
| FinalFields       | object       | 最终字段         |
| TransactionIndex  | unsigned int | 交易序号         |
| TransactionResult | string       | 交易结果         |
| delivered_amount  | string       | 交易金额         |
| validated         | bool         | 验证状态         |

## 6 查询交易详情

Request:

```
{
    "method": "tx",
    "params": [
        {
            "transaction":"B8AB124DB3214D95D34C771A626F3890E2099F53E43CC1B49BDF2C1D6A541100",
            "binary": false
        }
    ]
}

```

| 字段名称    | 类型   | 描述                                                                                                 |
| :---------- | :----- | :--------------------------------------------------------------------------------------------------- |
| method      | string | RPC 接口名称                                                                                         |
| transaction | string | 交易记录的 hash，即交易的流水号                                                                      |
| binary      | bool   | 交易结果是否以二进制显示，默认 false。如果为 true 则以二进制格式显示；若为 false，则以 JSON 格式显示 |

Response:

```
{
    "result": {
        "Account": "MTCwjBNR5YBoQQMign51ED59sTkx1DhtiiTu",
        "Amount": "100",
        "Destination": "MTCGqLp9dri37aME12EMa4qrAMDoPdFjaW8M",
        "Fee": "100",
        "Flags": 2147483648,
        "Sequence": 12,
        "SigningPubKey": "0354F197E0FB1CE445F532DDB83339F20CADE87A21ACF45E9C5A79697CC2E16A9E",
        "TransactionType": "Payment",
        "TxnSignature": "3045022100E1B350782C8ABEAED12D3709B6C6289B16AF4166E9668CE5DF134E052A03302402201813C2D4979D3E0A6BA2AD6F7F994BDFE4691D2647CBB08B222C94EF454720A0",
        "date": 654836430,
        "hash": "B8AB124DB3214D95D34C771A626F3890E2099F53E43CC1B49BDF2C1D6A541100",
        "inLedger": 17466,
        "ledger_current_index": 17481,
        "ledger_index": 17466,
        "meta": {
            "AffectedNodes": [
                {
                    "ModifiedNode": {
                        "FinalFields": {
                            "Account": "MTCwjBNR5YBoQQMign51ED59sTkx1DhtiiTu",
                            "Balance": "9999074195872",
                            "Flags": 0,
                            "Issues": [
                                "757EBEDEF84C121BF42D74673577CD13B0921DA6752732FBF7AD9E01DBA1C3B0",
                                "1B57F8092C1B718299ED8840D9A825F2E86A6A22B6C0A2E233F04D7E0E9E356C"
                            ],
                            "OwnerCount": 2,
                            "Sequence": 13
                        },
                        "LedgerEntryType": "AccountRoot",
                        "LedgerIndex": "4D616F2243FBBD36E1C768F6124C311205844E60EE702CE3F033B2E63119FD7E",
                        "PreviousFields": {
                            "Balance": "9999074196072",
                            "Sequence": 12
                        },
                        "PreviousTxnID": "684CC1262FA186EF34E7992D220671F23BA4DDD6D56213EE07197357027431E2",
                        "PreviousTxnLgrSeq": 10206
                    }
                },
                {
                    "ModifiedNode": {
                        "FinalFields": {
                            "Account": "MTCGqLp9dri37aME12EMa4qrAMDoPdFjaW8M",
                            "Balance": "999989900",
                            "Flags": 8388608,
                            "Issues": [
                                "D5BFA859E9DFA1D66637ADFFB6A1646C4C0F42FFC19FBDBEEAF2BF102E7F02D4"
                            ],
                            "OwnerCount": 0,
                            "Sequence": 4
                        },
                        "LedgerEntryType": "AccountRoot",
                        "LedgerIndex": "AD333501F4523A69A1ED1EEDBA436068718ED8FE230DFD8A1FDF707E7CC12D53",
                        "PreviousFields": {
                            "Balance": "999989800"
                        },
                        "PreviousTxnID": "8972363F01CC7B2398405B8A0799622B92AFEEDD98C67542773014D4E8A35BE6",
                        "PreviousTxnLgrSeq": 491
                    }
                },
                {
                    "ModifiedNode": {
                        "FinalFields": {
                            "Account": "MTCJNfG2CPaNUx8hgDj5EZMceWUuyVM9ga8B",
                            "Balance": "72300",
                            "Flags": 0,
                            "OwnerCount": 0,
                            "Sequence": 1
                        },
                        "LedgerEntryType": "AccountRoot",
                        "LedgerIndex": "EDC80B6A0C901F9EA3B1987AAEDED5690185AFA838066DE94A60E842440717EF",
                        "PreviousFields": {
                            "Balance": "72200"
                        },
                        "PreviousTxnID": "844C8596B35E55C0909C0020FA04F90D75A8D2D20F1DC7AEAE6D52DEF780BF66",
                        "PreviousTxnLgrSeq": 10227
                    }
                }
            ],
            "TransactionIndex": 0,
            "TransactionResult": "tesSUCCESS",
            "delivered_amount": "100"
        },
        "status": "success",
        "validated": true
    }
}
```

| 字段名称          | 类型         | 描述                 |
| :---------------- | :----------- | :------------------- |
| date              | unsigned int | 交易时间             |
| ledger_index      | unsigned int | 交易所在账本的账本号 |
| meta              | object       | 交易记录原始信息     |
| AffectedNodes     | object       | 影响的单元           |
| ModifiedNode      | object       | 修改的单元           |
| FinalFields       | object       | 最终字段             |
| TransactionIndex  | unsigned int | 交易序号             |
| TransactionResult | string       | 交易结果             |
| delivered_amount  | string       | 交易金额             |
| validated         | bool         | 验证状态             |

## 7 获取当前已验证区块高度

Request:

```
{
    "method": "ledger",
    "params": [
        {
            "ledger_index": "validated",
            "accounts": false,
            "full": false,
            "transactions": false,
            "expand": false,
            "owner_funds": false
        }
    ]
}
```

| 字段名称     | 类型                | 描述                                                                                                                                                                                  |
| :----------- | :------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| ledger_hash  | string              | 账本哈希                                                                                                                                                                              |
| ledger_index | unsignedint，object | 账本高度                                                                                                                                                                              |
| full         | bool                | （可选）如果 true，返回整个分类帐的完整信息。如果未指定分类帐版本，则忽略。默认为 false。（相当于启用 transactions，accounts 和 expand。）警告：这是一个非常大量的数据 - 大约几百兆！ |
| accounts     | bool                | （可选）如果 true，返回分类帐中账户的信息。如果未指定分类帐版本，则忽略。默认为 false。警告：这会返回大量数据！                                                                       |
| transactions | bool                | （可选）如果 true，返回指定分类帐版本中的交易信息。默认为 false。如果未指定分类帐版本，则忽略                                                                                         |
| expand       | bool                | （可选）为交易/账户信息提供完整的 JSON 格式信息，而不是仅提供哈希值。默认为 false。除非您请求交易，账户或两者，否则忽略                                                               |
| owner_funds  | bool                | （可选）如果 true，owner_funds 在响应中包含 OfferCreate 交易的元数据中的字段。默认为 false。除非包含交易并且 expand 为真，否则忽略                                                    |
| binary       | bool                | （可选）如果 true，以二进制格式（十六进制字符串）而不是 JSON 格式返回交易信息                                                                                                         |
| queue        | bool                | （可选）如果 true，则在结果中包含一系列排队交易                                                                                                                                       |

Response:

```
{
    "result": {
        "ledger": {
            "accepted": true,
            "account_hash": "A999B4C8C2A23878CDD600832C7E0D334B38888B0946AD4256135B6AEE555691",
            "close_flags": 0,
            "close_time": 654836650,
            "close_time_human": "2020-Oct-01 03:04:10",
            "close_time_resolution": 10,
            "closed": true,
            "fee_pool": "0",
            "hash": "CA1AD5C67F2DD3F97102A008569F686675C786FD5A4467027E0547885BBD8EE7",
            "ledger_hash": "CA1AD5C67F2DD3F97102A008569F686675C786FD5A4467027E0547885BBD8EE7",
            "ledger_index": "17488",
            "parent_close_time": 654836640,
            "parent_hash": "7D89A23762A2E334757EE24A99D1CEE939DEB84C16DFA78A66924A8121032730",
            "seqNum": "17488",
            "totalCoins": "21000000000000",
            "total_coins": "21000000000000",
            "transaction_hash": "0000000000000000000000000000000000000000000000000000000000000000"
        },
        "ledger_hash": "CA1AD5C67F2DD3F97102A008569F686675C786FD5A4467027E0547885BBD8EE7",
        "ledger_index": 17488,
        "status": "success",
        "validated": true
    }
}
```

| 字段名称                     | 类型   | 描述                                                     |
| :--------------------------- | :----- | :------------------------------------------------------- |
| ledger                       | object | 支付执行结果标题                                         |
| ledger.account_hash          | string | 此分类帐中所有账户状态信息的十六进制哈希值               |
| ledger.accountState          | array  | （除非已请求，否则将被忽略）此分类帐中的所有账户状态信息 |
| ledger.close_flags           | int    | 与关闭此分类帐相关的标志位                               |
| ledger.close_time            | int    | 此分类账关闭的时间                                       |
| ledger.close_time_human      | string | 此分类账以人类可读的格式关闭的时间                       |
| ledger.close_time_resolution | int    | 帳本关闭时间在这么多秒内四舍五入                         |
| ledger.closed                | bool   | 此分类账是否已经关闭                                     |
| ledger.ledger_hash           | string | 整个分类帐的唯一标识哈希                                 |
| ledger.ledger_index          | string | 此分类帐的分类帐索引，作为带引号的整数                   |
| ledger.parent_close_time     | int    | 前一个分类帐关闭的时间                                   |
| ledger.parent_hash           | string | 紧接在此之前的分类帐的唯一标识哈希                       |
| ledger.total_coins           | string | 网络中 M 的总数，作为带引号的整数                      |
| ledger.transaction_hash      | string | 此分类帐中包含的交易信息的十六进制哈希值                 |
| ledger_hash                  | string | 整个分类帐的唯一标识哈希                                 |
| ledger_index                 | number | 此分类帐的分类帐索引                                     |



