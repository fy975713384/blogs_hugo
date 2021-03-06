# 一种基于 PDF 解析的合同自动化测试系统

## 摘要

本发明适用于软件测试技术领域，提供了一种对业务过程中所签署的合同进行自动化测试的方法。该方法包括：基于业务数据状态进行的合同数量验证。其中，业务数据各种业务状态下预期应签署的合同列表已在系统中进行了配置。当合同文件数量验证完成后，系统记录验证状态，并提取到已签署的合同 PDF 文件用于进行后续测试。以及基于 PDF 解析技术的合同文件内容要素验证。合同要素包括：合同内的业务数据、合同电子签章等，系统会将合同要素信息与系统中配置好的合同要素预期结果进行比对验证，并记录验证结果。上述验证完成后，系统将提取全部验证结果，将验证不通过的内容进行自动缺陷上报，并自动生成测试报告，完成合同自动化测试过程。

## 技术领域

本发明涉及计算机技术领域，特别涉及一种基于 PDF 解析的合同自动化测试系统。

## 背景技术

随着国内互联网技术的高速发展，电子合同已逐渐渗透到各行各业。特别是在相关政策的引导下，电子合同在互联网金融、在线旅游、制造零售等行业已成为合规“标配”。

由于合同本身具有一定的法律效力，所签署电子合同数量以及内容的正确性对于企业来说至关重要。但当前对于电子合同的测试大多还处于手工测试阶段，自动化程度很低，使用手工的方式进行电子合同测试主要存在以下弊端：

一是手工测试存在天然的效率低下问题，而快速的业务需求变化又使得电子合同的改动十分频繁，即使投入大量的人力资源也难以为继；

二是电子合同测试内容多，首先是在某些领域的业务过程中，需要签署的合同数量较多，对于稍微复杂一些的合同，仅需要配置的业务数据可能就多达数十项，手工测试难免会产生遗漏；

三是电子合同涉及较多的数字计算，尤其是在金融领域，通过手工的方式无论是借助计算器计算还是脑力计算，都显得过程繁琐且容易出错。

## 发明内容

针对上述问题，本发明希望提供一种通用的可配置的合同自动化测试系统，以解决传统技术的各种弊端，提升电子合同作为一种软件的质量，解决企业痛点。其关键特征如下：

一、该系统实现了一个合同预期结果配置组件，需要配置的内容包括：业务数据状态，与业务数据状态相对应的预期应签署的合同列表，与合同种类相对应的预期应存在的要素标识，与要素标识相对应的内容固定的要素信息。配置内容的存储方式可以采用关系型数据库（如：MySQL、Oracle 等），配置文件（如：TOML、YAML、INI 等）等。并且该组件应该可以便捷地进行配置内容的提取。

二、该系统实现了一个文件处理组件，基于超文本传输协议提供文件下载功能，用于下载合同 PDF 文件到系统指定的合同 PDF 文件存储位置；同时提供两种文件存储方式，一是存储服务器，在文件数量较多且占用内存空间较大时建议选择该方式，二是本地存储，在文件数量较少或占用内存空间较少时可以将 PDF 文件直接存放于当前项目下的特定路径下。

三、该系统提供了一个 PDF 文件解析组件，基于 PDF 文件解析技术提供包括 PDF 文本信息提取、PDF 图片信息提取、PDF 表格信息提取、PDF 签章信息提取等功能，并可根据业务中具体使用的技术定制化底层功能实现。

四、该系统提供了一个包含多个校验器的合同校验组件，其中每个校验器都可根据实际需要进行启用或禁用，包括但不限于合同文件数量校验器，合同文本内容校验器，合同自定义参数校验器，合同图片校验器，合同表格内容校验器，合同签章内容校验器。

五、该系统实现了一个测试结果处理组件，包括一个测试结果收集器，并提供测试缺陷自动上报功能、测试报告自动生成功能、测试报告发送功能。其中测试缺陷自动上报功能开放底层代码实现，需要对企业具体采用的缺陷管理工具进行适配。

六、该系统实现了多个合同多个字段的同时测试。基于多进程技术，并行地开展多个业务数据的同时测试；通过多线程技术，并发地进行同一份合同的多个字段的同时测试，可成倍地提升合同测试效率。

## 具体实施方式

第一步，系统接收外部传入的业务数据唯一标识（以下简称为业务标识），在业务数据中应该可通过该业务标识确定唯一的业务数据，并获取到该业务数据的全部合同相关信息（通过直接查询或关联查询获取均可）。

第二步，系统根据上一步传入的业务标识自动判断其业务数据状态，该业务数据状态属于系统配置的业务数据状态的一种。

第三步，系统根据当前业务数据状态自动从系统配置中提取预期应签署的合同列表，从业务数据中提取实际已签署的合同列表，一并传入合同文件数量校验器。合同文件数量校验器会对传入的数据进行精确的比对校验，完成后将结果记录到测试结果收集器中。验证不通过的数据完成记录后，不阻塞流程，继续下一步验证。

第四步，提取所有已签署的合同 PDF 文件，且这些合同都应该在配置的预期签署合同列表中。

第五步，一个个地将上一步提取出的合同 PDF 文件传入 PDF 文件解析组件中。

第六步， PDF 文件解析组件对传入的合同 PDF 文件进行解析。

第七步，将解析后的内容和系统配置的预期结果传入启用的验证器中，进行比对验证。

第八步，每一个合同要素信息验证完成后，实时将测试结果记录到测试结果收集器中，验证不通过的也不阻塞流程。

第九步，测试结果处理组件提取分析测试过程中记录的测试结果，并自动进行汇总生成测试报告发送给配置的测试报告接收者。其中，测试不通过的信息将通过异步方式自动进行缺陷的提交上报。
