<template>
    <div class="recent_tx">
        <div class="header">
            <div class="left">
                <h2>Latest Transactions</h2>
                <v-btn :loading="isAjax" :text="true" @click="updateTx" class="refresh">
                    <fa icon="sync"></fa>
                    <span class="ava-btn-label">Refresh</span>
                </v-btn>
            </div>
            <p class="chain right">
                <span class="label">You are viewing transactions for</span>
                <v-tooltip bottom>
                    <template v-slot:activator="{ on }">
                        <span v-on="on" class="tag">AVA X-Chain</span>
                    </template>
                    <span>The X-Chain acts as a decentralized platform for creating and trading smart digital assets. (Think X for eXchanging assets.)</span>
                </v-tooltip>
            </p>
        </div>
        <div class="list">
            <div class="table_headers recent_tx_rows">
                <p></p>
                <p>
                    ID
                    <v-tooltip bottom>
                        <template v-slot:activator="{ on }">
                            <fa
                                v-on="on"
                                icon="info-circle"
                                transform="shrink-6"
                                :style="{ color: '#e8e7ea' }"
                            ></fa>
                        </template>
                        <span>a transaction queries or modifies the state of a blockchain</span>
                    </v-tooltip>
                </p>
                <p>
                    From
                    <v-tooltip bottom>
                        <template v-slot:activator="{ on }">
                            <fa
                                v-on="on"
                                icon="info-circle"
                                transform="shrink-6"
                                :style="{ color: '#e8e7ea' }"
                            ></fa>
                        </template>
                        <span>address that sends transfer value</span>
                    </v-tooltip>
                </p>
                <p>
                    To
                    <v-tooltip bottom>
                        <template v-slot:activator="{ on }">
                            <fa
                                v-on="on"
                                icon="info-circle"
                                transform="shrink-6"
                                :style="{ color: '#e8e7ea' }"
                            ></fa>
                        </template>
                        <span>address that receives transfer value</span>
                    </v-tooltip>
                </p>
            </div>
            <transition-group name="fade">
                <tx-row
                    v-for="tx in transactions"
                    :key="tx.id"
                    class="recent_tx_rows"
                    :transaction="tx"
                ></tx-row>
            </transition-group>
        </div>
        <div class="bottom">
            <router-link to="/tx" class="view_all">View All transactions</router-link>
        </div>
    </div>
</template>
<script>
import api from "@/axios";
import Vue from "vue";

import TxRow from "@/components/rows/TxRow/TxRow";

export default Vue.extend({
    components: {
        TxRow
    },
    data() {
        return {
            isAjax: false,
            // all_tx: [],
            transactions: []
        };
    },
    created() {
        this.updateTx();
    },
    methods: {
        updateTx() {
            const parent = this;
            let txNum = 8;
            this.isAjax = true;
            api.get(`/x/transactions?sort=timestamp-desc&limit=${txNum}`).then(
                res => {
                    const list = res.data.transactions;
                    parent.transactions = list;
                    parent.isAjax = false;
                }
            );
        }
    }
});
</script>
<style scoped lang="scss">
@use '../../main';

.refresh {
    margin-left: 16px;
}

.ava-btn-label {
    padding-left: 8px;
}

.table_headers {
    display: grid;
    grid-template-columns: 35px 120px 1fr 1fr;
    padding-bottom: 7px;
    border-bottom: 1px solid #e7e7e7;

    p {
        padding: 0px 10px;
        font-weight: bold;
    }
}

.col_1 {
    padding: 0px 30px;
}

.header {
    display: flex;
    align-items: center;
    margin-bottom: 20px;

    h2 {
        padding-bottom: 2px;
    }

    .refresh {
        font-size: 12px;
        text-transform: none;
        border: none;
        opacity: 0.5;
    }

    .chain {
        font-size: 12px;
        color: #929ba6;
        font-weight: lighter;
        text-align: right;
        flex-grow: 1;

        .label {
            padding: 4px 12px 4px 0;
            min-height: 1em;
            line-height: 2em;
        }

        .tag {
            padding: 4px 12px;
            border-radius: 4px;
            color: #976cfa;
            background-color: #ebe4fb;
            min-height: 1em;
            line-height: 2em;
            word-break: keep-all;
            white-space: nowrap;
        }
    }

    .left {
        display: flex;
        justify-content: flex-start;
        align-items: center;
    }
}

.recent_tx_rows {
    width: 100%;
    font-size: 12px;
    border-radius: 2px;
    margin-bottom: 2px;
    box-sizing: border-box;
    border-bottom: 1px solid #e7e7e7;
}

.bottom {
    display: flex;
    flex-flow: row-reverse;
}

.view_all {
    display: block;
    width: max-content;
    text-decoration: none !important;
    margin-top: 30px;
    background-color: main.$primary-color;
    color: #fff !important;
    padding: 12px 24px;
    font-size: 12px;
    border-radius: 4px;
}

.fade-enter-active,
.fade-leave-active {
    transition: opacity 1s;
}

@include main.device_s {
    .view_all {
        width: 100%;
        text-align: center;
    }

    .table_headers {
        display: none;
    }

    .header {
        padding-bottom: 0;
    }

    .list {
        padding: 0;
    }
}

@include main.device_xs {
    .header {
        display: flex;
        flex-direction: column;

        .left {
            display: flex;
            width: 100%;
            justify-content: space-between;
            margin-bottom: 5px;
        }

        .right {
            width: 100%;
            display: flex;
            flex-direction: row;
            justify-content: flex-start;
            align-content: center;
        }
    }
}
</style>
