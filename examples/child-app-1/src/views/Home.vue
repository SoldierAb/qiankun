<template>
  <div class="content">
    <button primary @click="modifyConf">修改配置数据</button>
    <input v-model="address" />
    <p>conf.baseURL -> {{ conf.baseURL }}</p>
    <p>conf.appId -> {{ conf.appId }}</p>
  </div>
</template>

<script>
export default {
  name: 'Page1',
  data() {
    return {
      address: '',
      conf: V.conf,
      visible: false,
    };
  },
  mounted() {
    const { address } = this;
    V.ajax({
      method: 'POST',
      url: '/api/example/route/' + address,
      data: {
        data: { module: 'common', action: 'get_UTC_time' },
        game_id: 104,
      },
      headers: {
        Token: V.conf.Authorization,
      },
    });
  },
  methods: {
    modifyConf() {
      const { address } = this;
      V.conf.setState({
        baseURL: address,
      });
      if (!Object.prototype.hasOwnProperty.call(V.conf, 'Authorization')) {
        V.conf.add('Authorization', `Bearer ${address}`);
      } else {
        V.conf.setState({
          Authorization: `Bearer ${address}`,
        });
      }
      localStorage.setItem('NAME', address);
      setTimeout(() => {
        console.log('child-app-1 [NAME] ', localStorage.getItem('NAME'));
      });
      V.ajax({
        method: 'POST',
        url: '/api/example/route/' + address,
        data: {
          data: { module: 'common', action: 'get_UTC_time' },
          game_id: 104,
        },
        headers: {
          Token: V.conf.Authorization,
        },
      });
    },
  },
};
</script>

<style lang="css" scoped>
.content {
  color: red;
}
</style>
