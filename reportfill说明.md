# 传参
现在传的参数有四个：<br>
  1、companyReportId： CompanyReport表的id；<br>
  2、companyPeriodReportStatusId： CompanyPeriodReportStatus表的id，同时也是CompanyReport表的字段；<br>
  3、action： 要进行的操作是什么，包括edit编辑，view查看，添加和修改同属于edit操作；<br>
  4、isAccu： 是否累计，包括 0 否， 1 是。<br>
# 获取下一级操作级别的方法（位于report项目想的common.conreoller里）
  1、保存草稿<br>
   <pre><code>
    @RequestMapping(value = "/saveDraft/getNextStatus/{companyReportId}", method = RequestMethod.GET)
    @ResponseBody
    public int getNextStatusWhenSaveDraft(@PathVariable("companyReportId") String companyReportId) {
        int newStatus = 0;
        try {
            CompanyReport companyReport = reportFillService.getCompanyReport(companyReportId);
            int oldStatus = companyReport.getStatus();
            switch (oldStatus) {
                case 101:
                case 201:
                case 202:
                    newStatus = 201;
                    break;
                case 203:
                case 204:
                case 205:
                    newStatus = 204;
                    break;
            }
            return newStatus;
        } catch (Exception e) {
            logger.error(e);
            return newStatus;
        }
    }
   </code></pre>
  2、保存
  <pre><code>
    @RequestMapping(value = "/save/getNextStatus/{companyReportId}", method = RequestMethod.GET)
    @ResponseBody
    public int getNextStatusWhenSave(@PathVariable("companyReportId") String companyReportId) {
        int newStatus = 0;
        try {
            CompanyReport companyReport = reportFillService.getCompanyReport(companyReportId);
            int oldStatus = companyReport.getStatus();
            switch (oldStatus) {
                case 101:
                case 201:
                case 202:
                    newStatus = 202;
                    break;
                case 203:
                case 204:
                case 205:
                    newStatus = 205;
                    break;
            }
            return newStatus;
        } catch (Exception e) {
            logger.error(e);
            return newStatus;
        }
    }
    </code></pre>
  3、说明<br>
    参数就是传一个companyReportId就可以，如果返回 0 的话，应该就是报错了，可以在前台判断一下。
      
# 更新CompanyReport表中的status
  <pre><code>
    @Autowired
    ReportFillService reportFillService;
    reportFillService.updateCompanyRport(companyReport);
  </code></pre>
  注：传companyReport实体类中必须包含companyReportId和status<br>
     返回值是Boolean类型，true更新成功，false更新失败
       
# 更新CompanyReport表和CompanyPeriodReportStatus表中的status
  <pre><code>
    @Autowired
    ReportFillService reportFillService;
    reportFillService.updateCompanyRportAndCompanyPeriodReportStatus(companyReport);
  </code></pre>
   注：传的companyReport实体类中必须包括companyReportId和status以及companyPeriodReportStatusId<br>
        返回值是Boolean类型，true更新成功，false更新失败
