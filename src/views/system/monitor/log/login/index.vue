<script setup lang="tsx">
import type { TableColumn } from '@/components/core/dynamic-table'
import { Alert, Tag } from 'ant-design-vue'
import dayjs from 'dayjs'
import Api from '@/api/'
import { useTable } from '@/components/core/dynamic-table'
import { formatToDateTime } from '@/utils/dateUtil'

defineOptions({
  name: 'SystemMonitorLoginLog',
})

const [DynamicTable] = useTable()

const columns: TableColumn<API.LoginLogInfo>[] = [
  {
    title: 'ID',
    dataIndex: 'id',
    width: 60,
    hideInSearch: true,
  },
  {
    title: '用户名',
    dataIndex: 'username',
    width: 120,
  },
  {
    title: 'IP',
    dataIndex: 'ip',
    width: 80,
    customRender: ({ record }) => {
      return <Tag color="blue">{record.ip}</Tag>
    },
  },
  {
    title: '登录地点',
    dataIndex: 'address',
    width: 80,
  },
  {
    title: '登录时间',
    width: 120,
    dataIndex: 'time',
    customRender({ record }) {
      return formatToDateTime(record.time)
    },
    formItemProps: {
      component: 'RangePicker',
      componentProps: {
        valueFormat: 'YYYY-MM-DD HH:mm:ss',
      },
      transform([startDate, endDate] = []) {
        if (startDate && endDate) {
          return [
            dayjs(startDate).startOf('day').format('YYYY-MM-DD HH:mm:ss'),
            dayjs(startDate).endOf('day').format('YYYY-MM-DD HH:mm:ss'),
          ]
        }
      },
    },
  },
  {
    title: '操作系统',
    dataIndex: 'os',
    width: 120,
    hideInSearch: true,
  },
  {
    title: '浏览器',
    dataIndex: 'browser',
    width: 120,
    hideInSearch: true,
  },
]
</script>

<template>
  <div id="auto-height" style="height: 80%">
    <DynamicTable
      header-title="登录日志"
      auto-height="#auto-height"
      :data-request="Api.systemLog.logLoginLogPage"
      :columns="columns"
    />
  </div>
</template>
