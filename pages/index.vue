<template>
  <v-layout
    column
    justify-center
    align-center
  >
    <v-flex
      xs12
      sm8
      md6
    >
      <v-card>
        <v-tabs
          v-model="currentItem"
          background-color="transparent"
          color="basil"
          grow
        >
          <v-tab
            v-for="item in tabs"
            :key="item"
            :href="'#tab-' + item"
          >
            {{ item }}
          </v-tab>
        </v-tabs>
        <v-tabs-items v-model="currentItem">
          <v-tab-item
            v-for="item in tabs"
            :key="item"
            :value="'tab-' + item"
          >
            <v-card v-if="currentItem == 'tab-Респонденты'" flat d-flex pa-2>
              <v-card-title class="headline">
                Добавить опрос
              </v-card-title>
              <v-card-text>
                <v-container>
                <v-row align="center">
                  <v-col cols="4">
                    <v-subheader>Условие {{counter}}</v-subheader>
                  </v-col>
                  <v-col cols="8">
                    <v-select
                      v-model="select"
                      :hint="`${select.quest}`"
                      :items="items"
                      item-text="quest"
                      item-value="num"
                      label="Select"
                      persistent-hint
                      return-object
                      single-line
                    ></v-select>
                  </v-col>
                </v-row>
                <v-col cols="12" v-if="select.num == 1">
                    <v-row v-for="(range, index) in ranges" v-bind:key="range.id">
                      <v-col cols="4" style="display:flex;align-items:center;">
                        Диапазон {{index + 1}}
                      </v-col>
                      <v-col cols="4">
                          <v-text-field
                            v-model="range.from"
                            :rules="rangeRules"
                            :counter="range.from"
                            :key="index"
                            label="от"
                            required
                          ></v-text-field>
                      </v-col>
                      <v-col cols="4">
                          <button @click="deleteRange(index)" style="position:absolute;right:5%">x</button>
                          <v-text-field
                            v-model="range.before"
                            :rules="rangeRules"
                            :counter="range.before"
                            :key="index"
                            label="до"
                            required
                          ></v-text-field>
                      </v-col>
                    </v-row>
                    <v-row>
                      <v-col cols="4"></v-col>
                      <v-col cols="8">
                        <v-btn large color="primary" @click="addRange()">Добавить диапазон</v-btn>
                      </v-col>
                    </v-row>
                </v-col>
                <v-row v-else style="margin: 0 12px">
                    <v-col cols="12">
                      {{select.prop}} 
                    </v-col>
                </v-row>
                </v-container>
              </v-card-text>
            </v-card>
            <v-card v-else flat>
              <v-card-text>
                <h2>{{ item }}</h2>
                {{ text }}
              </v-card-text>
            </v-card>
          </v-tab-item>
        </v-tabs-items>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>

export default {
  components: {

  },
  data () {
    return {
      counter: 1,
      currentItem: 'tab-Респонденты',
      select: 
      { 
        quest: 'Возраст респондента', 
        num: 1, 
        prop: 'Диапазон',
        range: [
          { c: 1, from: 0, before: 0 },
        ],
      },
      items: [
          { quest: 'Возраст респондента', num: 1, prop: 'Диапазон',},
          { quest: 'Тип карты лояльности', num: 2, prop: 'Тип' },
          { quest: 'Статус карты лояльности', num: 3, prop: 'Активна' },
      ],
      ranges: [
        { from: 0, before: 0 },
      ],
      tabs: [
        'Параметры', 'Вопросы', 'Логика', 'Условия', 'Респонденты',
      ],
      text: 'Контент вкладки контент вкладки контент вкладки контент вкладки контент вкладки...',
      from: '',
      before: '',
      rangeRules: [
        v => !!v || 'Обязательное поле',
        v => v.length <= 3 || 'Диапазон может быть от 20 до 100',
      ],
    }
  },
  computed: {

  },
  methods: {
    addRange() {
       this.ranges.push({ from: 0, before: 0 });
    },
    deleteRange: function (index) {
      console.log(index);
      this.ranges.splice(index, 1);
    },
  },
}
</script>
