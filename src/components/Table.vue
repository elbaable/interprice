<template>
    <table class="w-full text-sm text-left text-gray-500 dark:text-gray-400 border-2">
        <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
            <tr>
                <th scope="col" class="py-3 px-6 uppercase">
                    <div class="flex items-center">
                        Date sent
                        <a href="#" @click="sortByDateAndFlag()"><svg xmlns="http://www.w3.org/2000/svg" class="ml-1 w-3 h-3" aria-hidden="true"
                                fill="currentColor" viewBox="0 0 320 512">
                                <path
                                    d="M27.66 224h264.7c24.6 0 36.89-29.78 19.54-47.12l-132.3-136.8c-5.406-5.406-12.47-8.107-19.53-8.107c-7.055 0-14.09 2.701-19.45 8.107L8.119 176.9C-9.229 194.2 3.055 224 27.66 224zM292.3 288H27.66c-24.6 0-36.89 29.77-19.54 47.12l132.5 136.8C145.9 477.3 152.1 480 160 480c7.053 0 14.12-2.703 19.53-8.109l132.3-136.8C329.2 317.8 316.9 288 292.3 288z">
                                </path>
                            </svg></a>
                    </div>
                </th>
                <th scope="col" class="py-3 px-6 uppercase">
                    <div class="flex items-center">
                        Company
                        <a href="#" @click="sortByCompanyName()"><svg xmlns="http://www.w3.org/2000/svg"
                                class="ml-1 w-3 h-3" aria-hidden="true" fill="currentColor" viewBox="0 0 320 512">
                                <path
                                    d="M27.66 224h264.7c24.6 0 36.89-29.78 19.54-47.12l-132.3-136.8c-5.406-5.406-12.47-8.107-19.53-8.107c-7.055 0-14.09 2.701-19.45 8.107L8.119 176.9C-9.229 194.2 3.055 224 27.66 224zM292.3 288H27.66c-24.6 0-36.89 29.77-19.54 47.12l132.5 136.8C145.9 477.3 152.1 480 160 480c7.053 0 14.12-2.703 19.53-8.109l132.3-136.8C329.2 317.8 316.9 288 292.3 288z">
                                </path>
                            </svg></a>
                    </div>
                </th>
                <th scope="col" class="py-3 px-6" rowspan="2" v-if="years.includes(5)">
                    <td class="flex items-center justify-center border-b-2 border-neutral-400" colspan="2">
                        5 yrs
                    </td>
                    <td class="flex justify-around">
                        <span>FIX</span>
                        <span>FRN</span>
                    </td>
                </th>
                <th scope="col" class="py-3 px-6" rowspan="2" v-if="years.includes(10)">
                    <td class="flex items-center justify-center border-b-2 border-neutral-400" colspan="2">
                        10 yrs
                    </td>
                    <td class="flex justify-around">
                        <span>FIX</span>
                        <span>FRN</span>
                    </td>
                </th>
                <th scope="col" class="py-3 px-6" rowspan="2" v-if="years.includes(40)">
                    <td class="flex items-center justify-center border-b-2 border-neutral-400" colspan="2">
                        40 yrs
                    </td>
                    <td class="flex justify-around">
                        <span>FIX</span>
                        <span>FRN</span>
                    </td>
                </th>
            </tr>
        </thead>
        <tbody v-for="(company, index) in companyData" :key="company.id">
            <tr class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 cursor-pointer"
                @click="toggle(company.id)" :class="{ opened: opened.includes(company.id) }">
                <td scope="row" class="py-4 px-6 font-medium text-gray-900 whitespace-nowrap dark:text-white">
                    <font-awesome-icon v-if="company.quote != null && !opened.includes(company.id)"
                        icon="fa-solid fa-chevron-right" class="mr-2" />
                    <font-awesome-icon v-if="company.quote != null && opened.includes(company.id)"
                        icon="fa-solid fa-chevron-down" class="mr-2" />
                    {{company.date}}
                </td>
                <td class="py-4 px-6" :class="{'font-bold': company.quote != null}">
                    {{company?.name}}
                </td>
                <td class="py-4 px-6 text-center" v-if="years.includes(5)">
                    <span class="float-left w-1/2" :class="{ highlight: checkMin(index, 5) }">
                        {{getMainField(company.quote, 5, "FIX", currentFieldArr[0])}}
                    </span>
                    <span class="float-right w-1/2">
                        {{getMainField(company.quote, 5, "FRN", currentFieldArr[0])}}
                    </span>
                </td>
                <td class="py-4 px-6 text-center" v-if="years.includes(10)">
                    <span class="float-left w-1/2" :class="{ highlight: checkMin(index, 10) }">
                        {{getMainField(company.quote, 10, "FIX", currentFieldArr[0])}}
                    </span>
                    <span class="float-right w-1/2">
                        {{getMainField(company.quote, 10, "FRN", currentFieldArr[0])}}
                    </span>
                </td>
                <td class="py-4 px-6 text-center" v-if="years.includes(40)">
                    <span class="float-left w-1/2" :class="{ highlight: checkMin(index, 40) }">
                        {{getMainField(company.quote, 40, "FIX", currentFieldArr[0])}}
                    </span>
                    <span class="float-right w-1/2">
                        {{getMainField(company.quote, 40, "FRN", currentFieldArr[0])}}
                    </span>
                </td>
            </tr>
            <tr v-if="company.quote != null && opened.includes(company.id)"
                class="bg-white border-b dark:bg-gray-800 dark:border-gray-700">
                <td scope="row" class="py-4 px-6 font-medium text-gray-900 whitespace-nowrap dark:text-white">

                </td>
                <td class="py-4 px-6">
                    {{currentFieldArr[1]}}
                </td>
                <td class="py-4 px-6 text-center" v-if="years.includes(5)">
                    <span class="float-left w-1/2">
                        {{getMainField(company.quote, 5, "FIX", currentFieldArr[1])}}
                    </span>
                    <span class="float-right w-1/2">
                        {{getMainField(company.quote, 5, "FRN", currentFieldArr[1])}}
                    </span>
                </td>
                <td class="py-4 px-6 text-center" v-if="years.includes(10)">
                    <span class="float-left w-1/2">
                        {{getMainField(company.quote, 10, "FIX", currentFieldArr[1])}}
                    </span>
                    <span class="float-right w-1/2">
                        {{getMainField(company.quote, 10, "FRN", currentFieldArr[1])}}
                    </span>
                </td>
                <td class="py-4 px-6 text-center" v-if="years.includes(40)">
                    <span class="float-left w-1/2">
                        {{getMainField(company.quote, 40, "FIX", currentFieldArr[1])}}
                    </span>
                    <span class="float-right w-1/2">
                        {{getMainField(company.quote, 40, "FRN", currentFieldArr[1])}}
                    </span>
                </td>
            </tr>
            <tr v-if="company.quote != null && opened.includes(company.id)"
                class="bg-white border-b dark:bg-gray-800 dark:border-gray-700">
                <td scope="row" class="py-4 px-6 font-medium text-gray-900 whitespace-nowrap dark:text-white">

                </td>
                <td class="py-4 px-6">
                    {{currentFieldArr[2]}}
                </td>
                <td class="py-4 px-6 text-center" v-if="years.includes(5)">
                    <span class="float-left w-1/2">
                        {{getMainField(company.quote, 5, "FIX", currentFieldArr[2])}}
                    </span>
                    <span class="float-right w-1/2">
                        {{getMainField(company.quote, 5, "FRN", currentFieldArr[2])}}
                    </span>
                </td>
                <td class="py-4 px-6 text-center" v-if="years.includes(10)">
                    <span class="float-left w-1/2">
                        {{getMainField(company.quote, 10, "FIX", currentFieldArr[2])}}
                    </span>
                    <span class="float-right w-1/2">
                        {{getMainField(company.quote, 10, "FRN", currentFieldArr[2])}}
                    </span>
                </td>
                <td class="py-4 px-6 text-center" v-if="years.includes(40)">
                    <span class="float-left w-1/2">
                        {{getMainField(company.quote, 40, "FIX", currentFieldArr[2])}}
                    </span>
                    <span class="float-right w-1/2">
                        {{getMainField(company.quote, 40, "FRN", currentFieldArr[2])}}
                    </span>
                </td>
            </tr>
        </tbody>
        <tfoot>
            <tr>
                <td></td>
                <td class="py-3">Average by {{currentFieldArr[0]}}</td>
                <td v-if="years.includes(5)" class="py-3 text-center"><span class="float-left w-1/2">{{getAverage(5, "FIX")}}</span><span class="float-right w-1/2">{{getAverage(5, "FRN")}}</span>
                </td>
                <td v-if="years.includes(10)" class="py-3 text-center"><span class="float-left w-1/2">{{getAverage(10, "FIX")}}</span><span class="float-right w-1/2">{{getAverage(10,
                "FRN")}}</span></td>
                <td v-if="years.includes(40)" class="py-3 text-center"><span class="float-left w-1/2">{{getAverage(40, "FIX")}}</span><span class="float-right w-1/2">{{getAverage(40,
                "FRN")}}</span></td>
            </tr>
        </tfoot>
    </table>
</template>

<script>
export default {
    name: "DataTable",
    props: {
        data: Array,
        opened: Array,
        years: Array,
        currency: String,
        toggle: Function,
        field: String
    },
    watch: {
        field: {
            handler: function () {
                this.convertCompanyInfo()
            }
        }
    },
    created() {
        this.convertCompanyInfo()
    },
    data() {
        return {
            companyData: this.data,
            currentFieldArr: [],
            nameSort: true,
            dateSort: true
        };
    },
    methods: {
        convertCompanyInfo() {
            this.currentFieldArr.length = 0;
            var fieldArr = ["Spread", "Yield", "3MLSpread"];
            this.currentFieldArr.push(this.field);
            const index = fieldArr.indexOf(this.field)
            fieldArr.splice(index, 1)
            this.currentFieldArr.push(...fieldArr)
        },
        getMainField(quote, year, coupon, selectedField) {
            if (selectedField == "Spread") {
                return this.getSpread(quote?.filter(e => e.Years == year && e.Currency == this.currency && e.CouponType == coupon)[0]?.Spread)
            } else if (selectedField == "Yield") {
                return this.getYield(quote?.filter(e => e.Years == year && e.Currency == this.currency && e.CouponType == coupon)[0]?.Yield)
            } else {
                return this.get3MLSpread(quote?.filter(e => e.Years == year && e.Currency == this.currency && e.CouponType == coupon)[0])
            }
        },
        getSpread(val) {
            if (val == undefined || val == null) {
                return "";
            }
            return "+" + val + "bp";
        },
        getYield(val) {
            if (val == undefined || val == null) {
                return "";
            }
            return val + "%";
        },
        get3MLSpread(val) {
            if (val != undefined && val["3MLSpread"] != null) {
                return "+" + val["3MLSpread"] + "bp";
            }
            return "";
        },
        getAverage(year, coupon) {
            var average = 0;
            var count = 0;
            this.companyData.map(item => {
                var quote = item.quote?.filter(e => e.Years == year && e.CouponType == coupon)[0]
                if (quote != undefined) {
                    average += quote[this.currentFieldArr[0]];
                    count++;
                }
            })
            if (average == 0) {
                return "";
            }
            if (this.currentFieldArr[0] == "Yield") {
                return parseFloat(average / count).toFixed(3) + "%";
            } else {
                return "+" + parseFloat(average / count).toFixed(0) + "bp"
            }
        },
        sortByDateAndFlag() {
            this.companyData.sort((a, b) => {
                if(this.dateSort) {
                    return new Date(b.date ?? "1970-1-1") - new Date(a.date ?? "1970-1-1")
                } else {
                    return new Date(a.date ?? "1970-1-1") - new Date(b.date ?? "1970-1-1")
                }
            })
            // this.companyData.sort((a, b) => {
            //     if(this.dateSort) {
            //         if(a.preferred < b.preferred) {
            //             return -1;
            //         }
            //         if(a.preferred > b.preferred) {
            //             return 1;
            //         }
            //     } else {
            //         if(a.preferred > b.preferred) {
            //             return -1;
            //         }
            //         if(a.preferred < b.preferred) {
            //             return 1;
            //         }
            //     }
            //     return 0;
            // })
            
            this.dateSort = !this.dateSort;
        },
        sortByCompanyName() {
            this.companyData.sort((a, b) => {
                const nameA = a.name.toUpperCase();
                const nameB = b.name.toUpperCase();
                
                if (this.nameSort) {
                    if (nameA < nameB) {
                        return -1;
                    }
                    if (nameA > nameB) {
                        return 1;
                    }
                } else {
                    if (nameA > nameB) {
                        return -1;
                    }
                    if (nameA < nameB) {
                        return 1;
                    }
                }
                return 0;
            })
            this.nameSort = !this.nameSort;
        },
        checkMin(id, year) {
            var filterArr = this.companyData.map((item) => {
                var quote = item.quote?.filter(e => e.Years == year && e.CouponType == "FIX")
                if (quote != undefined && quote.length > 0) {
                    return quote[0];
                }
            })
            var val = filterArr[id];
            if(val == undefined) {
                return false;
            }
            var compVal = val[this.currentFieldArr[0]];
            var isMin = true;
            for(var i = 0; i < filterArr.length; i++) {
                if(id == i) {
                    continue;
                }
                if(filterArr[i] == undefined) {
                    continue;
                }
                var nextVal = filterArr[i][this.currentFieldArr[0]];
                if(nextVal != null && nextVal < compVal) {
                    isMin = false;
                    break;
                }
            }
            return isMin;
        }
    }
}
</script>

<style scoped>
    .highlight {
        background-color: #fff08f;
    }
</style>