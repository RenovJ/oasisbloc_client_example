<template>
  <div class="requester">
    <h1>{{ title }}</h1>
    <div>
      <h2>Configurations</h2>
        <b-form-group
          id="input-group-1-1"
          label="Blockchain access point"
          label-for="input-1-1"
          label-cols-sm="4"
          label-cols-lg="3"
        >
          <b-form-input
            id="input-1-1"
            v-model="config.blockchain_access_point"
            type="text"
            required
            placeholder="ex) http://127.0.0.1:7777"
          ></b-form-input>
        </b-form-group>
        
        <b-form-group
          id="input-group-1-2"
          label="Blockchain chain id"
          label-for="input-1-2"
          label-cols-sm="4"
          label-cols-lg="3"
        >
          <b-form-input
            id="input-1-2"
            v-model="config.blockchain_chain_id"
            type="text"
            required
            placeholder=""
          ></b-form-input>
        </b-form-group>
        
        <b-form-group
          id="input-group-1-3"
          label="Data trade contract account name"
          label-for="input-1-3"
          label-cols-sm="4"
          label-cols-lg="3"
        >
          <b-form-input
            id="input-1-3"
            v-model="config.data_trade_contract_account_name"
            type="text"
            required
            placeholder="ex) data.trading"
          ></b-form-input>
        </b-form-group>
        
        <b-form-group
          id="input-group-1-4"
          label="User account name"
          label-for="input-1-4"
          label-cols-sm="4"
          label-cols-lg="3"
        >
          <b-form-input
            id="input-1-4"
            v-model="config.user_account_name"
            type="text"
            required
            placeholder=""
          ></b-form-input>
        </b-form-group>
        
        <b-form-group
          id="input-group-1-5"
          label="User account private key"
          label-for="input-1-5"
          label-cols-sm="4"
          label-cols-lg="3"
        >
          <b-form-input
            id="input-1-5"
            v-model="config.user_account_private_key"
            type="text"
            required
            placeholder=""
          ></b-form-input>
        </b-form-group>
        <div style="text-align:right;">
          <b-button id="btnConnectBlockchain" type="submit" variant="primary" v-on:click="connectBlockchain">{{ connectionStatus }}</b-button>
          <b-button type="reset" variant="danger">Reset</b-button>
        </div>
    </div>
    
    
    <b-form-group v-if="osb" label="Trading data">
      <b-form-radio-group id="radio-group-2" v-model="behavior" name="radio-sub-component">
        <b-form-radio :value="BEHAVIOR_PROVIDING">Providing data</b-form-radio>
        <b-form-radio :value="BEHAVIOR_BUYING">Buying data</b-form-radio>
      </b-form-radio-group>
    </b-form-group>
    
    <div v-if="behavior === BEHAVIOR_PROVIDING">
      <h2>Providing data</h2>
      <b-form @submit="onSubmit_1" @reset="onReset_1">
      
        <b-form-group
          id="input-group-2-1"
          label="Datatypename"
          label-for="input-2-1"
          label-cols-sm="4"
          label-cols-lg="3"
        >
          <b-input-group>
            <b-form-input
              id="input-2-1"
              v-model="providing_data.datatypename"
              type="text"
              required
            ></b-form-input>
            <b-input-group-append>
              <b-button variant="secondary" v-on:click="getDetailFields">Get</b-button>
            </b-input-group-append>
          </b-input-group>
        </b-form-group>
        
        <div v-for="(fieldname, index) in providing_data.detailFieldsName">
          <b-form-group 
            :label="fieldname"
            :label-for="'input-2-2-' + index"
            label-cols-sm="4"
            label-cols-lg="3"
          >
            <b-form-input
              :id="'input-2-2' + index"
              v-model="providing_data.detailFields[index]"
              type="text"
              required
            ></b-form-input>
          </b-form-group>
        </div>
        
        <b-form-group
          v-if="false"
          label="Data type"
          label-cols-sm="4"
          label-cols-lg="3"
          name="data_type"
          v-model="providing_data.data_type">
          <b-form-radio-group
            v-model="providing_data.data_type"
            :options="dataTypeOptions"
            name="radio-inline"
          ></b-form-radio-group>
        </b-form-group>

        <b-form-group
          label="Data"
          label-for="input-2-5"
          label-cols-sm="4"
          label-cols-lg="3"
        >
          <b-form-textarea
            v-if="providing_data.data_type === 'text'"
            id="input-2-5"
            v-model="providing_data.data_text"
            required
          ></b-form-textarea>

          <b-form-file
            v-else
            v-model="providing_data.data_file"
            id="input-2-6"
            placeholder="Choose a file..."
            drop-placeholder="Drop file here..."
          ></b-form-file>
        </b-form-group>
        
        <b-form-group
          id="input-group-2-7"
          label="Price (OSB, decimal: 4)"
          label-for="input-2-7"
          label-cols-sm="4"
          label-cols-lg="3"
        >
          <b-form-input
            id="input-2-7"
            v-model="providing_data.price"
            type="text"
            required
            placeholder="ex) 99.0000"
          ></b-form-input>
        </b-form-group>
        
        <b-form-group
          id="input-group-2-8"
          label="Period (day)"
          label-for="input-2-8"
          label-cols-sm="4"
          label-cols-lg="3"
        >
          <b-form-input
            id="input-2-8"
            v-model="providing_data.period"
            type="number"
            required
          ></b-form-input>
        </b-form-group>
        
        <b-form-group
          id="input-group-2-9"
          label="Decrypt key 1"
          label-for="input-2-9"
          label-cols-sm="4"
          label-cols-lg="3"
        >
          <b-form-input
            id="input-2-9"
            v-model="providing_data.decryptKeyList[0]"
            type="text"
            required
          ></b-form-input>
        </b-form-group>
        
        <b-form-group
          id="input-group-2-10"
          label="Decrypt key 2"
          label-for="input-2-10"
          label-cols-sm="4"
          label-cols-lg="3"
        >
          <b-form-input
            id="input-2-10"
            v-model="providing_data.decryptKeyList[1]"
            type="text"
            required
          ></b-form-input>
        </b-form-group>
      
      
        <div style="text-align:right;">
          <b-button type="submit" variant="primary">Submit</b-button>
          <b-button type="reset" variant="danger">Reset</b-button>
        </div>
      </b-form>
    </div>

    <div v-if="behavior === BEHAVIOR_BUYING">
      <h2>Buying data</h2>
      <b-form @submit="onSubmit_2" @reset="onReset_2">      
        <b-form-group
          id="input-group-3-1"
          label="Reserved data ID"
          label-for="input-3-1"
          label-cols-sm="4"
          label-cols-lg="3"
        >
          <b-form-input
            id="input-3-1"
            v-model="buying_data.reserved_data_id"
            type="number"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group
          id="input-group-3-2"
          label="Buyer private key"
          label-for="input-3-2"
          label-cols-sm="4"
          label-cols-lg="3"
        >
          <b-form-input
            id="input-3-2"
            v-model="buying_data.buyer_private_key"
            type="text"
            required
          ></b-form-input>
        </b-form-group>

        <div style="text-align:right;">
          <b-button type="submit" variant="primary">Submit</b-button>
          <b-button type="reset" variant="danger">Reset</b-button>
        </div>
      </b-form>
    </div>
    
    <b-card v-if="behavior !== 0" class="mt-3 mb-5" header="Result">
      <pre class="m-0">{{ response }}</pre>
    </b-card>

  </div>
</template>

<script src="//unpkg.com/vue"></script>
<script src="//unpkg.com/axios/dist/axios.min.js"></script>
<script>
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'
import OASIS_CLI from 'osb-cli-sdk'

const BEHAVIOR_PROVIDING = 1
const BEHAVIOR_BUYING = 2

export default {
  name: 'Requester',
  data () {
    return {
      title: 'An example of web application using OASISBloc client sdk',
      BEHAVIOR_PROVIDING: BEHAVIOR_PROVIDING,
      BEHAVIOR_BUYING: BEHAVIOR_BUYING,
      config: {
        blockchain_access_point: 'http://172.16.1.12:8888',
        blockchain_chain_id: '201414dff180ea57066f6e7baf27f178ad6cc3f22d52506c34227bf188033d86',
        data_trade_contract_account_name: 'datatrader23',
        user_account_name: 'alice1111111',
        user_account_private_key: '5JA9CDA7Pg6ovEz3hQTBXQ6xWTcAmcEmGU6RY6yDaKEp6bnTtkN',
      },
      providing_data: {
        datatypename: '',
        detailFieldsName: [],
        detailFields: [],
        data_type: 'text',
        data_text: '',
        data_file: '',
        price: '',
        period: '',
        decryptKeyList: [
        '',
        ''
        ],
      },
      buying_data: {
        reserved_data_id: '',
        buyer_private_key: '',
      },
      response: '',
      dataTypeOptions: [
        { text: 'Text', value: 'text' },
        { text: 'File', value: 'file' }
      ],
      osb: 0,
      connectionStatus: 'Connect',
      behavior: 0,
    }
  },
  methods: {
    connectBlockchain () {
      const config = {
        chainAccessPoint: this.config.blockchain_access_point,
        chainId: this.config.blockchain_chain_id,
        dataTradeContractName: this.config.data_trade_contract_account_name,
        privateKey: this.config.user_account_private_key,
        account: this.config.user_account_name
      }
      this.osb = new OASIS_CLI(config)
      this.osb.setEventCallback(this.osbCallback)
      
      if (this.osb) {
        this.connectionStatus = 'Connected'
      }
    },
    async onSubmit_1(evt) {
      evt.preventDefault()
      var data
      if (this.providing_data.data_type === 'text') {
        data = this.providing_data.data_text
      } else if (this.providing_data.data_type === 'file') {
        data = this.providing_data.data_file
      }
      data = Buffer.from(data)
      
      const returnedData = await this.osb.registerData(
        data,
        this.providing_data.datatypename,
        this.providing_data.detailFields,
        this.providing_data.price, 
        this.providing_data.period, 
        this.providing_data.decryptKeyList)
        
      this.response += '\n' + JSON.stringify(returnedData)
    },
    onReset_1(evt) {
      evt.preventDefault()
        this.providing_data.datatypename= ''
        this.providing_data.data_type= ''
        this.providing_data.data_text= 'text'
        this.providing_data.data_file= ''
        this.providing_data.price= ''
        this.providing_data.period= ''
    },
    async onSubmit_2(evt) {
      evt.preventDefault()
      
      const returnedData = await this.osb.buyData(this.buying_data.reserved_data_id, this.buying_data.buyer_private_key)
      this.response += '\n' + JSON.stringify(returnedData)
      
      const acquiredData = await this.osb.requestData(this.buying_data.reserved_data_id, returnedData.dataBuyHistoryInfo.buy_id, this.buying_data.buyer_private_key)
    },
    onReset_2(evt) {
      evt.preventDefault()

        this.buying_data.reserved_data_id = ''
        this.buying_data.buyer_private_key = ''
    },
    osbCallback(msg) {
      if (typeof msg === 'object') {
        this.response+= '\n' + JSON.stringify(msg) + '\n'
      } else {
        this.response+= '\n' + msg
      }
    },
    async getDetailFields () {
      const datatypelist = await this.osb.eosjs.getTableRows({
            json: true,
            code: this.config.data_trade_contract_account_name,
            scope: this.config.data_trade_contract_account_name,
            table: "datatype",
            table_key: "datatype_id",
            limit: 30
        }).catch(function(error) {
            console.log(error)
            // reject('failed to retrieve datatype list')
        });
        
      if (!datatypelist) {
        console.log('error - failed to retrieve datatype list')
        return
      }
      
      for (var i = 0; i < datatypelist.rows.length; i++) {
        if (datatypelist.rows[i].datatype_name === this.providing_data.datatypename) {
          this.providing_data.detailFieldsName = datatypelist.rows[i].detail_fields
        }
      }
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
  margin-top: 50px;
}
a {
  color: #42b983;
}
.requester {
  max-width: 800px;
  margin:auto;
}
</style>
