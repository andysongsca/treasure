<template>
  <div class="block">
    <h1 class="demonstration">华宇工作日计算截止日期</h1>
    <mu-date-picker
      hintText="选择一个开始时间"
      autoOk
      labelFloat
      label="选择一个开始时间"
      inputClass="startDp"
      @change="handleChangeDate"
      :shouldDisableDate="disableRandomDates"
      />
    <h2>输入工作日天数</h2>
    <el-input-number v-model="workDayNum" @change="handleChangeDayNum" :min="1"></el-input-number>
    <h3 class="result">{{result}}</h3>
  </div>
</template>

<script>
export default {
    name: 'HelloWorld',
    data () {
        return {
            startDate: '',
            defaultDate: new Date().toString(),
            workDayNum: 1,
            result: '',
            holiday: ['2018-1-1', '2018-2-12', '2018-2-13', '2018-2-14', '2018-2-15', '2018-2-16', '2018-2-19', '2018-2-20', '2018-2-21', '2018-2-22', '2018-2-23', '2018-4-5', '2018-4-6', '2018-4-30', '2018-5-1', '2018-6-18', '2018-9-24', '2018-10-1', '2018-10-2', '2018-10-3', '2018-10-4', '2018-10-5', '2018-12-31'],
            weekendsOff: ['2018-2-10', '2018-4-8','2018-4-28','2018-9-29','2018-9-30','2018-12-29']
        }
    },
    methods: {
        handleChangeDayNum (value) {
            this.workDayNum = value
            this.result = this.getEndDate(this.startDate, this.workDayNum)
        },
        handleChangeDate (value) {
            this.startDate =  this.formatDate(new Date(value));
            this.result = this.getEndDate(this.startDate, this.workDayNum)
        },
        /**
         *
         * @param {*} startDay 开始日期 ’2018-1-1‘
         * @param {*} workDays 工作日天数
         * @return 结束日期 ’2018-1-1‘
         */
        getEndDate(startDay, workDays) {
            if (!Date.parse(startDay)) {
                alert('输入时间格式有误！');
                return;
            }
            if (!Number.isInteger(workDays) || +workDays < 0) {
                alert('输入工作日天数有误');
                return;
            }
            var dateArr = [];
            var firtDay = new Date(startDay);
            var countDays = 0;
            var endDate = null;
            var dayNum = 0

            while (workDays !== countDays) {
                endDate = new Date(firtDay.getTime() + dayNum * 1000 * 60 * 60 * 24);
                if (endDate.getDay() > 0 && endDate.getDay() < 6 && !this.isHoliday(endDate) || ((endDate.getDay() === 0 || endDate.getDay() === 6) && this.isWeekendsOff(endDate))) {
                    countDays ++;
                }
                dayNum ++;
            }
            return this.formatDate(endDate);
        },
        disableRandomDates(endDate) {
          return !(endDate.getDay() > 0 && endDate.getDay() < 6 && !this.isHoliday(endDate) || ((endDate.getDay() === 0 || endDate.getDay() === 6) && this.isWeekendsOff(endDate)))
        },
        /**
         * 判断某天是否是节假日
         * @param {Date} date 日期对象
         * @return Boolean
         */
        isHoliday(date) {
            var isHoliday = false;
            this.holiday.forEach(function(v) {
                var temp = v.replace(/-/g, '/');
                var wo = new Date(temp);
                if (wo.getTime() === date.getTime()) {
                    isHoliday = true;
                }
            });
            return isHoliday;
        },
        /**
         * 判断某天是否是调休日
         * @param {Date} date
         * @return Boolean
         */
        isWeekendsOff(date) {
            var isWeekendsOff = false;
            this.weekendsOff.forEach(function(v) {
                var temp = v.replace(/-/g, '/');
                var wo = new Date(temp);
                if (wo.getTime() === date.getTime()) {
                    isWeekendsOff = true;
                }
            });
            return isWeekendsOff;
        },
        /**
         * 格式化时间
         * @param {Date} d
         * @return 'yyyy-mm-dd'
         */
        formatDate(d) {
            var month = '' + (d.getMonth() + 1);
            var day = '' + d.getDate();
            var year = d.getFullYear();

            // if (month.length < 2) month = '0' + month;
            // if (day.length < 2) day = '0' + day;

            return [year, month, day].join('/');
        }

    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.result {
    margin: 50px auto;
    height: 90%;
    line-height: 50px;
    color: #fff;
    border-radius: 25px;
    background-color: #67c23a;
}
.startDp {
  text-align: center;
}
</style>
