## Index Table
| belong | api name | description |
| --- | --- | --- |
| <vktiolib/vankia.trans.hpp> | withdraw| Transfer the assets from the current contract internal account to the multiple vkt main chain account. |
| <vktiolib/vankia.trans.hpp> | deposit| Transfer an vkt main chain account to the assets of the current contract  |
| <vktiolib/vankia.trans.hpp> | recordwithdraw| Record the transaction of the withdrawal |
| <vktiolib/vankia.trans.hpp> | getassets| Obtain an asset balance from an external account |
| <vktiolib/vankia.trans.hpp> | authrightreg| The authentication interface is used to authenticate the data corresponding to the incoming hash value |
| <vktiolib/action.h> | has_auth |  Verifies that name exists in the set of provided auths on a action. Throws if not found.  |
| <vktiolib/action.h> | send_inline | Send an inline action in the context of this action's parent transaction |
| <vktiolib/crypto.h> | sha256 | sha256 for calculating data |
| <vktiolib/crypto.h> | sha512 | sha512 for calculating data |
| <vktiolib/system.h> | print | Print for logging when debugging |
|     |     |     |

### example1
```c++
#include <vktiolib/action.h>
#include <vktiolib/contract.hpp>
#include <vktiolib/dispatcher.hpp>
#include <vktiolib/types.h>

using namespace vankia.trans;

class helloworld : public contract
{
public:
helloworld(uint64_t id) 
    : contract(id)
{   
}   

//@abi action
//@abi payable
void deposit(account_name from)
{
        accounts from_acnts( _contract, from);
        if( has_auth( from_acnts) ) {
            deposit(from_acnts, “1000.000 VKT”);
        }else{
            eosio_assert( false, "insufficient authority" );
        }
};

VKTLIB_ABI(helloworld, ())
```
Calling this contract through the wallet client
call_contract helloworld action deposit to account.
Call helloworld's deposit method, calling deposit() in the implementation of the deposit method will Perform the transfer {"amount":1000.0000,"token type":VKT} .

## void  withdraw(account_name from, vector<account_record_content> content)

include: <vktiolib/asset.hpp>
#include  "vankia.trans.hpp"

desc: Transfer the assets from the current contract internal account to the multiple vkt main chain account.

**params:**

<uint64_t> from: From which account on the main chain, usually _self

vector<account_record_content> content: 

<uint64_t> asset_id: When transferring the assets from a contract internal to the multiple accounts of the main chain, you need to Add in the the vector<account_record_content> object of users transferred to .

vector<account_record_content> content:The account_record_content type contains the username of the main chain and the asset content of the transfer.

The Account_record_content structure description:
```c++
struct  account_record_content
{
asset assets;
account_name person_name;
VKTLIB_SERIALIZE(account_record_content, (assets)(person_name))
};
```
<asset> assets: Transfer amount, this number contains the accuracy of the asset, for example, if you want to transfer 1 VKT, you should write "1.0000 VKT"
<account_name> person_name:The VKT main chain account name, for example, qingzhu123, the username must be 5-12 digits, and contains the letters [a-z] case insensitive, numbers [1-5].


## void deposit(account_name from, asset assets)

include: <vktiolib/asset.hpp>
#include  "vankia.trans.hpp"

desc: Transfer an vkt main chain account to the assets of the current contract .

**params:**

<account_name> from: From which account on the main chain, usually _self

<asset> assets:Transfer to specifies the amount of the asset and token type, this number contains the accuracy of the asset, for example, if you want to transfer 1 VKT, you should write "1.0000 VKT"


## void  recordwithdraw(account_name from, uint64_t seq, string hash, vector<account_record_content> trans_list)

include: <vktiolib/asset.hpp>
#include  "vankia.trans.hpp"

desc: Record the transaction of the withdrawal

**params:**

<account_name> from:From which account on the main chain, usually _self

<uint64_t > id: Serial number, the maximum is 18446744073709551616

<string> hash: If there is a withdrawal certificate for the IPFS record, please fill in

vector<account_record_content> trans_list:Record transactions from internal contracts to multiple accounts in the main chain

## asset getassets(account_name owner)

include: <vktiolib/asset.hpp>
#include  "vankia.trans.hpp"

desc: Obtain an asset balance from an external account

**params:**

<account_name > account: the name of the account on the vkt main chain

<asset> assets: specifies the amount of the asset and token type


## void authrightreg(const  uint64_t id, const account_name agent, const string ipfsvalue, const string memo, const account_name producer)

include: <vktiolib/asset.hpp>
#include  "vankia.authright.cpp"

desc: The authentication interface is used to authenticate the data corresponding to the incoming hash value.

**params:**

<uint64_t > id: Serial number, the maximum is 18446744073709551616

<account_name>  agent:Account name of the parent company to which the authentication data belongs.

<const string> ipfsvalue:Authentication data, the hash value returned after uploading to the IPFS server.

<string> memo:Remarks, the details of the authentication can be described in detail.

<const account_name> producer:User to which authentication data belongs

## bool  has_auth( account_name name )

include  <vktiolib/system.h>

Desc: Verifies that account name exists in the set of provided auths on a action. Throws if not found.

**params:**

<account_name> name: name of the account to be verified

## void  send_inline(char  *serialized_action, size_t size)

include  <vktiolib/system.h>

Desc: Send an inline action in the context of this action's parent transaction

**params:**

<char  *> serialized_action:serialized action

<size_t> size:size of serialized action in bytes


## void sha256(char data, uint32_t length, const checksum256 * hash)

include: <vktiolib/crypto.h>

Desc: calculated sha256 of data


**params:**

<char> data: the first address of the string used to calculate sha256

<uint32_t> length: the length of the data string

<const checksum256 *> hash: out the reference sha256 used to store the calculation



## void sha512(char data, uint32_t length, const checksum512 * hash)

Include: <vktiolib/crypto.h>

Desc: sha512 for calculating data


**params:**

<char> data: the first address of the string used to calculate sha512

<uint32_t> length: the length of the data string

<const checksum512 *> hash: the reference sha512 used to store the calculation



## void print(const char* ptr)

include: <vktiolib/system.h>

Desc: Print for logging when debugging

**params:**

<const char*> ptr: 


