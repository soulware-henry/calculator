<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'IndexPage',

  setup() {
    const input = ref();

    const calculating = () => {
      const isNum = /[^.\d]/;
      const isNotNum = /[.\d]/;

      const numArray = input.value.split(isNum).map((n: string) => Number(n));
      const operArray = input.value.split(isNotNum).filter((v: string) => {
        return v != '';
      });
      const pushNum: number[] = [];
      const pushOper: string[] = [];
      let isMultify = false;

      while (true) {
        if (!isMultify) {
          const num = numArray.splice(0, 1);
          pushNum.push(num[0]);
        } else {
          isMultify = false;
        }
        const oper = operArray.splice(0, 1);
        pushOper.push(oper[0]);

        if (pushOper.includes('*')) {
          const num = numArray.splice(0, 1);
          const calculate = Number(pushNum.pop()) * num[0];
          pushNum.push(calculate);
          pushOper.pop();
          isMultify = true;
        }
        if (pushOper.includes('/')) {
          const num = numArray.splice(0, 1);
          const calculate = Number(pushNum.pop()) / num[0];
          pushNum.push(calculate);
          pushOper.pop();
          isMultify = true;
        }
        if (numArray.length == 0) break;
      }

      let nowNum = pushNum.shift();

      for (const num of pushNum) {
        const shift = pushOper.shift();
        if (String(shift) == '-') {
          nowNum = Number(nowNum) - num;
        } else if (String(shift) == '+') {
          nowNum = Number(nowNum) + num;
        }
      }

      input.value = nowNum;
    };

    const addInput = (i: string) => {
      const isFirstInput =
        (!input.value || input.value == '-') &&
        ['+', '*', '.', '/'].includes(i);
      const isOperInput =
        ['+', '*', '.', '/', '-'].includes(i) &&
        ['+', '*', '.', '/', '-'].includes(String(input.value).slice(-1));

      if (isFirstInput) input.value = '';
      else if (isOperInput) input.value = input.value?.slice(0, -1) + i;
      else if (!input.value) input.value = i;
      else if (input.value) input.value = input.value + i;
      else console.log('invalid Value');
    };

    return { input, calculating, addInput };
  },
});
</script>

<template>
  <div class="q-pa-md q-gutter-y-md column items-start">
    <q-input v-model="input" class="w-210" readonly dense />

    <div class="column gray">
      <q-btn-group class="q-my-sm">
        <q-btn color="secondary" class="w-105" @click="input = ''" label="c" />

        <q-btn color="secondary" @click="calculating" class="w-105" label="=" />
      </q-btn-group>
      <q-btn-group class="q-my-sm">
        <q-btn color="secondary" label="1" @click="addInput('1')" />
        <q-btn color="secondary" label="2" @click="addInput('2')" />
        <q-btn color="secondary" label="3" @click="addInput('3')" />
        <q-btn color="secondary" label="+" @click="addInput('+')" />
      </q-btn-group>
      <q-btn-group class="q-my-sm">
        <q-btn color="secondary" label="4" @click="addInput('4')" />
        <q-btn color="secondary" label="5" @click="addInput('5')" />
        <q-btn color="secondary" label="6" @click="addInput('6')" />
        <q-btn color="secondary" label="-" @click="addInput('-')" />
      </q-btn-group>
      <q-btn-group class="q-my-sm">
        <q-btn color="secondary" label="7" @click="addInput('7')" />
        <q-btn color="secondary" label="8" @click="addInput('8')" />
        <q-btn color="secondary" label="9" @click="addInput('9')" />
        <q-btn color="secondary" label="*" @click="addInput('*')" />
      </q-btn-group>
      <q-btn-group class="q-my-sm">
        <q-btn
          color="secondary"
          class="w-105"
          label="0"
          @click="addInput('0')"
        />

        <q-btn color="secondary" label="." @click="addInput('.')" />
        <q-btn color="secondary" label="/" @click="addInput('/')" />
      </q-btn-group>
    </div>
  </div>
</template>
<style lang="scss" scoped>
.gray {
  background-color: gray;
}
</style>
