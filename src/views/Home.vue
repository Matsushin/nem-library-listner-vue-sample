<template>
  <div class="home">
    <div v-if="startedListner == false">
      <h2>着金の監視</h2>
      <v-text-field 
        label="アドレス"
        v-model="address"
        :counter="40"
        arequired
        placeholder="例. NBHWRG6STRXL2FGLEEB2UOUCBAQ27OSGDTO44UFC"
      ></v-text-field>
      <v-flex>
        <v-btn color="blue" class="white-text" @click="startListner()">着金の監視を開始する</v-btn>
      </v-flex>
    </div>
    <div v-else>
      <h2 class="mb-2">着金情報</h2>
      <div v-if="message != ''">
        <v-alert
          :value="true"
          type="info"
        >
          <p>着金しました！</p>
          <p>{{message}}</p>
        </v-alert>
      </div>
      <div v-else>
        <p>着金監視中...</p>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import { Address, ConfirmedTransactionListener } from 'nem-library';

@Component({
  components: {

  },
})
export default class Home extends Vue {
  private address: string = '';
  private startedListner: boolean = false;
  private message: string = '';
 
  private async startListner() {
    const confirmedTransactionListener = new ConfirmedTransactionListener([
      {
        domain: '23.228.67.85'
      },
    ]).given(new Address(this.address));
    confirmedTransactionListener.subscribe((result) => {
      console.log(result);
      this.message = result;
    }, (err) => {
      console.log(err);
    });
    this.startedListner = true;
 }
}
</script>
