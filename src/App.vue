<template>
  <div>
    <b-tabs content-class="mt-3" card>
      <b-tab title="EI" active>
        <MyTable :idt="1" :table="ei" @delete="del" @add="add"></MyTable>
      </b-tab>
      <b-tab title="PROD">
        <MyTable :idt="2" :table="prod" @delete="del" @add="add"></MyTable>
      </b-tab>
      <b-tab title="SPEC_PROD">
        <MyTable :idt="3" :table="spec_prod" @delete="del" @add="add"></MyTable>
      </b-tab>
      <b-tab title="CHEM_CLASS">
        <MyTable :idt="4" :table="chem_class" @delete="del" @add="add"></MyTable>
      </b-tab>
      <b-tab title="Процедуры">
        <b-tabs content-class="mt-3" pills card vertical>
          <b-tab title="Процедура IN_GR" active>
            <b-input-group class="input-wrap mb-2">
              <span class="f">IN_GR(</span>
              <b-form-input class="input-arg" v-model="in_gr_params.pmaingr"></b-form-input>
              <span class="dot">,</span>
              <b-form-input class="input-arg" v-model="in_gr_params.pgr"></b-form-input>
              <span class="e">)</span>
              <b-button size="m" text="Добавить" variant="success" @click="in_gr()">Выполнить</b-button>
            </b-input-group>
            <MyTable v-if="in_gr_items" :table="in_gr_items" :procedure="true"></MyTable>
          </b-tab>
          <b-tab class="my-tab" title="Процедура SUM_Q">
            <b-input-group class="input-wrap mb-2">
              <span class="f">SUM_Q(</span>
              <b-form-input class="input-arg" v-model="sum_q_params.pidel"></b-form-input>
              <span class="dot">,</span>
              <b-form-input class="input-arg" v-model="sum_q_params.pq"></b-form-input>
              <span class="e">)</span>
              <b-button size="m" text="Добавить" variant="success" @click="sum_q()">Выполнить</b-button>
            </b-input-group>
            <MyTable v-if="sum_q_items" :table="sum_q_items" :procedure="true"></MyTable>
          </b-tab>
          <b-tab title="Процедура SUM_Q_F">
            <b-input-group class="input-wrap mb-2">
              <span class="f">SUM_Q_F(</span>
              <b-form-input class="input-arg" v-model="sum_q_f_params.pidel"></b-form-input>
              <span class="dot">,</span>
              <b-form-input class="input-arg" v-model="sum_q_f_params.pq"></b-form-input>
              <span class="dot">,</span>
              <b-form-input class="input-arg" v-model="sum_q_f_params.pgr"></b-form-input>
              <span class="e">)</span>
              <b-button size="m" text="Добавить" variant="success" @click="sum_q_f()">Выполнить</b-button>
            </b-input-group>
            <MyTable v-if="sum_q_f_items" :table="sum_q_f_items" :procedure="true"></MyTable>
          </b-tab>
        </b-tabs>
      </b-tab>
    </b-tabs>
  </div>
</template>

<script>
import MyTable from './components/MyTable.vue'

export default {
  name: 'App',
  components: {
    MyTable
  },
  data: () => ({
    ei: [  // единицы измерения
      {id_ei: 1, short_name: 'м', name: 'Метр', code: '006'},
      {id_ei: 2, short_name: 'кг', name: 'Килограмм', code: '166'},
      {id_ei: 3, short_name: 'т', name: 'Тонна', code: '168'},
      {id_ei: 5, short_name: 'шт', name: 'Штука', code: '796'}
    ],
    prod: [  // изделие 
      {id_prod: 2, short_name: "", name: "КРУГ 20-В СТ 40Х-1ТО", id_cl: 16, conf: 0, type_prod: null, flag: null, u: 2},
      {id_prod: 3, short_name: "", name: "КРУГ 20-В-НД СТ 20-2ГП-М1-ТВ1-КУВ-65", id_cl: 16, conf: 0, type_prod: null, flag: null, u: null},
      {id_prod: 4, short_name: "", name: "КРУГ 32-В-НД СТ 20-2ГП-М1-ТВ1-КУВ-65", id_cl: 16, conf: 0, type_prod: null, flag: null, u: null},
      {id_prod: 5, short_name: "", name: "КРУГ 36-И СТ 5ПС2", id_cl: 16, conf: 0, type_prod: null, flag: null, u: null},
      {id_prod: 6, short_name: "", name: "ПРОВОЛКА 1,2 СВ-08Г2С", id_cl: 17, conf: 0, type_prod: null, flag: null, u: null},
      {id_prod: 7, short_name: "", name: "ДВУОКИСЬ УГЛЕРОДА ЖИДКОГО ГОСТ 8050-85", id_cl: 18, conf: 0, type_prod: null, flag: null, u: null},
      {id_prod: 8, short_name: "", name: "КИСЛОТА СЕРНАЯ ТЕХН.КОНТ. 1С 2184-77", id_cl: 18, conf: 0, type_prod: null, flag: null, u: 2},
      {id_prod: 9, short_name: "", name: "КИСЛОТА АЗОТНАЯ КОНЦ.СОРТ-2 ГОСТ 701-89", id_cl: 18, conf: 0, type_prod: null, flag: null, u: 2},
      {id_prod: 10, short_name: "", name: "СТЕКЛО НАТРИЕВОЕ ЖИДКОЕ ГОСТ 13078-81", id_cl: 18, conf: 0, type_prod: null, flag: null, u: 2},
      {id_prod: 11, short_name: "", name: "НАТРИЙ СЕРНОКИСЛЫЙ КРИСТ. 1С А 4166-76", id_cl: 18, conf: 0, type_prod: null, flag: null, u: 2},
      {id_prod: 12, short_name: "", name: "НАТРИЙ ЕДКИЙ ТЕХН. ТВЕР.РТУТ. ГОСТ 2263-70", id_cl: 18, conf: 0, type_prod: null, flag: null, u: 2},
      {id_prod: 13, short_name: "", name: "БИХРОМАТ КАЛИЯ ТЕХН. 1С 2652-78", id_cl: 18, conf: 0, type_prod: null, flag: null, u: 2},
      {id_prod: 14, short_name: "", name: "ЖЕЛАТИН ФОТОГРАФ.,МАРКА Ф ТУ6-44-1548-9", id_cl: 18, conf: 0, type_prod: null, flag: null, u: 2},
      {id_prod: 15, short_name: "", name: "СОДА КАЛЬЦИНИРОВАН. СОРТ-1 ГОСТ 5100-85", id_cl: 18, conf: 0, type_prod: null, flag: null, u: 2},
      {id_prod: 16, short_name: "", name: "ТРИНАТРИЙФОСФАТ ГОСТ 201-76", id_cl: 18, conf: 0, type_prod: null, flag: null, u: 2},
      {id_prod: 17, short_name: "КП25.39.01.115-01", name: "ТРУБА", id_cl: 19, conf: 0, type_prod: null, flag: null, u: null},
      {id_prod: 18, short_name: "КП25.39.01.115", name: "ТРУБА", id_cl: 19, conf: 0, type_prod: null, flag: null, u: null},
      {id_prod: 19, short_name: "КП25.39.01.131", name: "ТРУБА Б/Ч", id_cl: 19, conf: 0, type_prod: null, flag: null, u: 2},
      {id_prod: 20, short_name: "", name: "ТРУБА 38*4 В20", id_cl: 19, conf: 0, type_prod: null, flag: null, u: null},
      {id_prod: 21, short_name: "", name: "ТРУБА 22*3 В20", id_cl: 19, conf: 0, type_prod: null, flag: null, u: 3},
      {id_prod: 22, short_name: "", name: "ШЕСТИГРАННИК 36 СТ 45-6-Т КАЛИБР.", id_cl: 20, conf: 0, type_prod: null, flag: null, u: 3},
      {id_prod: 23, short_name: "", name: "ШЕСТИГРАННИК 22 СТ 45-6-Т КАЛИБР.", id_cl: 20, conf: 0, type_prod: null, flag: null, u: 3},
      {id_prod: 24, short_name: "", name: "ПОЛИЭТИЛЕН 20908-040 ГОСТ 16338-85", id_cl: 21, conf: 0, type_prod: null, flag: null, u: 3},
      {id_prod: 25, short_name: "КП25.39.01.100", name: "ОРОСИТЕЛЬ", id_cl: 22, conf: 0, type_prod: null, flag: null, u: 0},
      {id_prod: 26, short_name: "КП25.39.01.120", name: "ОРОСИТЕЛЬ", id_cl: 22, conf: 0, type_prod: null, flag: null, u: null},
      {id_prod: 27, short_name: "КП25.39.01.110", name: "ОРОСИТЕЛЬ", id_cl: 22, conf: 0, type_prod: null, flag: null, u: null},
      {id_prod: 28, short_name: "КП31.01.46.030-09", name: "ФОРСУНКА", id_cl: 23, conf: 0, type_prod: null, flag: null, u: 1},
      {id_prod: 29, short_name: "", name: "ШАЙБА 20 65Г", id_cl: 24, conf: 0, type_prod: null, flag: null, u: 2},
      {id_prod: 30, short_name: "", name: "ШАЙБА 20 65Г ГОСТ 6402-70", id_cl: 24, conf: 0, type_prod: null, flag: null, u: 1},
      {id_prod: 31, short_name: "", name: "БОЛТ 3М20-6GX50.66", id_cl: 25, conf: 0, type_prod: null, flag: null, u: 1},
      {id_prod: 32, short_name: "КП25.39.01.113", name: "КОЖУХ", id_cl: 26, conf: 0, type_prod: null, flag: null, u: null},
      {id_prod: 33, short_name: "31.01.24.003-09", name: "КОРПУС", id_cl: 26, conf: 0, type_prod: null, flag: null, u: 2},
      {id_prod: 34, short_name: "КП25.39.01.101", name: "НАКЛАДКА", id_cl: 27, conf: 0, type_prod: null, flag: null, u: 1},
      {id_prod: 35, short_name: "", name: "ЛИСТ 30-Б-ПН-О СТ 3КП2", id_cl: 28, conf: 0, type_prod: null, flag: null, u: 2},
      {id_prod: 36, short_name: "", name: "ЛИСТ 36-Б-ПН-О СТ 3КП2", id_cl: 28, conf: 0, type_prod: null, flag: null, u: null},
      {id_prod: 37, short_name: "", name: "ЛИСТ 16-Б-ПН-О СТ 3КП2", id_cl: 28, conf: 0, type_prod: null, flag: null, u: null},
      {id_prod: 38, short_name: "", name: "ЛИСТ 45-Б-ПН-О СТ 3КП2", id_cl: 28, conf: 0, type_prod: null, flag: null, u: null},
      {id_prod: 39, short_name: "", name: "ЛИСТ 10-Б-ПН-О СТ 3КП2", id_cl: 28, conf: 0, type_prod: null, flag: null, u: null},
      {id_prod: 40, short_name: "", name: "ЛИСТ 45-Б-ПН-О СТ 5ПС2", id_cl: 28, conf: 0, type_prod: null, flag: null, u: null},
      {id_prod: 41, short_name: "КП25.39.01.130", name: "ТРУБОПРОВОД", id_cl: 29, conf: 0, type_prod: null, flag: null, u: 1},
      {id_prod: 42, short_name: "11.01.02.021-02", name: "ГАЙКА", id_cl: 30, conf: 0, type_prod: null, flag: null, u: 2},
      {id_prod: 43, short_name: "31.17.04.007-03", name: "ВКЛАДЫШ", id_cl: 31, conf: 0, type_prod: null, flag: null, u: 2}
    ],
    spec_prod: [ // спецификация изделия
      {id_pos: 1, id_prod: 30, num: 1, q: "0.013", flag: 0, use_el: 29},
      {id_pos: 2, id_prod: 31, num: 1, q: "0.002", flag: 0, use_el: 2},
      {id_pos: 3, id_prod: 34, num: 1, q: "0.005", flag: 0, use_el: 35},
      {id_pos: 4, id_prod: 41, num: 1, q: "1.000", flag: 0, use_el: 19},
      {id_pos: 5, id_prod: 41, num: 2, q: "2.000", flag: 0, use_el: 42},
      {id_pos: 6, id_prod: 28, num: 1, q: "0.002", flag: 0, use_el: 8},
      {id_pos: 7, id_prod: 28, num: 2, q: "0.001", flag: 0, use_el: 9},
      {id_pos: 8, id_prod: 28, num: 3, q: "0.001", flag: 0, use_el: 10},
      {id_pos: 9, id_prod: 28, num: 4, q: "0.001", flag: 0, use_el: 12},
      {id_pos: 10, id_prod: 28, num: 5, q: "0.001", flag: 0, use_el: 11},
      {id_pos: 11, id_prod: 28, num: 6, q: "0.001", flag: 0, use_el: 13},
      {id_pos: 12, id_prod: 28, num: 7, q: "0.001", flag: 0, use_el: 14},
      {id_pos: 13, id_prod: 28, num: 8, q: "0.001", flag: 0, use_el: 15},
      {id_pos: 14, id_prod: 28, num: 9, q: "0.001", flag: 0, use_el: 16},
      {id_pos: 15, id_prod: 28, num: 10, q: "1.000", flag: 0, use_el: 43},
      {id_pos: 16, id_prod: 28, num: 11, q: "1.000", flag: 0, use_el: 33},
      {id_pos: 17, id_prod: 25, num: 1, q: "6.000", flag: 0, use_el: 30},
      {id_pos: 18, id_prod: 25, num: 2, q: "6.000", flag: 0, use_el: 31},
      {id_pos: 19, id_prod: 25, num: 3, q: "2.000", flag: 0, use_el: 34},
      {id_pos: 20, id_prod: 25, num: 4, q: "8.000", flag: 0, use_el: 28},
      {id_pos: 21, id_prod: 25, num: 5, q: "1.000", flag: 0, use_el: 41},
      {id_pos: 22, id_prod: 19, num: 1, q: "0.505", flag: 0, use_el: 21},
      {id_pos: 23, id_prod: 42, num: 1, q: "0.002", flag: 0, use_el: 22},
      {id_pos: 24, id_prod: 43, num: 1, q: "0.003", flag: 0, use_el: 24},
      {id_pos: 25, id_prod: 33, num: 1, q: "0.001", flag: 0, use_el: 23}
    ],
    chem_class: [  // классификация изделия
      {id_class: 6, short_name: "", name: "Изделия", main_class: null, base_ei: null},
      {id_class: 14, short_name: "", name: "Материалы", main_class: 6, base_ei: 5},
      {id_class: 15, short_name: "", name: "Детали", main_class: 6, base_ei: 5},
      {id_class: 16, short_name: "", name: "Круг", main_class: 14, base_ei: 3},
      {id_class: 17, short_name: "", name: "Проволка", main_class: 14, base_ei: 2},
      {id_class: 18, short_name: "", name: "Вещества", main_class: 14, base_ei: 2},
      {id_class: 19, short_name: "", name: "Труба", main_class: 14, base_ei: 1},
      {id_class: 20, short_name: "", name: "Шестигранник", main_class: 14, base_ei: 3},
      {id_class: 21, short_name: "", name: "Полиэтилен", main_class: 14, base_ei: 2},
      {id_class: 22, short_name: "", name: "Ороситель", main_class: 15, base_ei: 5},
      {id_class: 23, short_name: "", name: "Форсунка", main_class: 15, base_ei: 5},
      {id_class: 24, short_name: "", name: "Шайба", main_class: 15, base_ei: 2},
      {id_class: 25, short_name: "", name: "Болт", main_class: 15, base_ei: 3},
      {id_class: 26, short_name: "", name: "Оболочка", main_class: 15, base_ei: 3},
      {id_class: 27, short_name: "", name: "Накладка", main_class: 15, base_ei: 5},
      {id_class: 28, short_name: "", name: "Лист", main_class: 14, base_ei: 3},
      {id_class: 29, short_name: "", name: "Тубопровод", main_class: 15, base_ei: 5},
      {id_class: 30, short_name: "", name: "Гайка", main_class: 15, base_ei: 5},
      {id_class: 31, short_name: "", name: "Вкладыш", main_class: 15, base_ei: 5}
    ],
    //in_gr
    in_gr_params: {
      pmaingr: null,
      pgr: null
    },
    in_gr_items: null,
    //sum_q
    sum_q_params: {
      pidel: null,
      pq: null
    },
    sum_q_items: null,
    //sum_q_f
    sum_q_f_params: {
      pidel: null,
      pq: null,
      pgr: null,
    },
    sum_q_f_items: null,

  }),
  methods: {
    add(props) {
      if (props.idt == 1) {
        this.ei.push(props.row)
      } else if (props.idt == 2) {
        this.prod.push(props.row)
      } else if (props.idt == 3) {
        this.spec_prod.push(props.row)
      } else if (props.idt == 4) {
        this.chem_class.push(props.row)
      }
    },
    del(props) {
      if (props.idt == 1 && this.ei.length > 1) {
        this.ei = this.ei.filter((e,i) => i != props.index)
      } else if (props.idt == 2 && this.prod.length > 1) {
        this.prod = this.prod.filter((e,i) => i != props.index)
      } else if (props.idt == 3 && this.spec_prod.length > 1) {
        this.spec_prod = this.spec_prod.filter((e,i) => i != props.index)
      } else if (props.idt == 4 && this.chem_class.length > 1) {
        this.chem_class = this.chem_class.filter((e,i) => i != props.index)
      }
    },
    in_gr() {
      let next_main_cl = this.in_gr_params.pmaingr
      let next_pgr = this.in_gr_params.pgr
      let oYes = 0

      let get_main_class = () => {
        let ch_cl = this.chem_class.find(ch_cl => ch_cl.id_class == next_pgr)
        return ch_cl ? ch_cl.main_class : null
      }

      next_main_cl = get_main_class()
      while (next_main_cl != null) {
        if (this.in_gr_params.pmaingr == next_main_cl) {
          oYes = 1
          break
        }
        next_pgr = next_main_cl
        next_main_cl = get_main_class()
      }
      
      this.in_gr_items = [{oYes}]
    },
    sum_q() {
      if (!this.sum_q_params.pidel || !this.sum_q_params.pq) return

      let d_els = []

      let f_depends_el = (id_el, q_el) => {
        this.spec_prod.forEach(sp => {
          if (sp.id_prod == id_el) {
            d_els.push([sp.use_el, sp.q * q_el])
            f_depends_el(sp.use_el, sp.q * q_el)
          }
        })
        return d_els
      }
      let sum_q_res = f_depends_el(this.sum_q_params.pidel, this.sum_q_params.pq)

      this.sum_q_items = []
      sum_q_res.forEach(r => {
        this.sum_q_items.push({OINEL: r[0], OQ: r[1].toFixed(3).toString()})
      })
    },
    sum_q_f() {
      if (!this.sum_q_f_params.pidel || !this.sum_q_f_params.pq || !this.sum_q_f_params.pgr) return

      let d_els = []

      let f_depends_el = (id_el, q_el) => {
        this.spec_prod.forEach(sp => {
          if (sp.id_prod == id_el) {
            d_els.push([sp.use_el, sp.q * q_el])
            f_depends_el(sp.use_el, sp.q * q_el)
          }
        })
        return d_els
      }
      let sum_q_res = f_depends_el(this.sum_q_f_params.pidel, this.sum_q_f_params.pq)

      this.sum_q_f_items = []
      sum_q_res.forEach(r => {
        let tmp = {}
        tmp.OINEL = r[0]
        tmp.OSUMQ = r[1].toFixed(3).toString()

        for (let i = 0; i < this.prod.length; i++) {
          let pr = this.prod[i]
          if (pr.id_prod == r[0]) {
            tmp.OSHORTNAMEEL = pr.short_name
            tmp.ONAMEEL = pr.name
            tmp.OIDGR = pr.id_cl
            break
          }
        }

        for (let i = 0; i < this.chem_class.length; i++) {
          let ch_cl = this.chem_class[i]
          if (ch_cl.id_class == tmp.OIDGR) {
            tmp.OIDEI = ch_cl.base_ei
            break
          }
        }

        for (let i = 0; i < this.ei.length; i++) {
          let e = this.ei[i]
          if (e.id_ei == tmp.OIDEI) {
            tmp.OSHORTNAMEEI = e.short_name
            break
          }
        }

        this.in_gr_params.pmaingr = this.sum_q_f_params.pgr
        this.in_gr_params.pgr = tmp.OIDGR
        this.in_gr()

        if (this.in_gr_items[0].oYes == 1) {
          
          for (let i = 0; i < this.chem_class.length; i++) {
            let ch_cl = this.chem_class[i]
            if (ch_cl.id_class == tmp.OIDGR) {
              tmp.ONAMEGR = ch_cl.name
              break
            }
          }
          
          this.sum_q_f_items.push(tmp)
        }
      })
    }
  }
}
</script>

<style>
.input-wrap span{
  align-self: center;
}
.dot{
  margin: 5px !important;
}
.input-arg {
  max-width: 100px;
}
.f {
  margin-right: 5px !important;
}
.e {
  margin: 0 5px !important;
}
.my-tab {
  justify-content: center;
}
</style>
