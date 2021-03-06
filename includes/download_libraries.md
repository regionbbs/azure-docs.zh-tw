## Azure Client Libraries for Java - 手動安裝
Azure Libraries for Java 依據 [Apache 授權 2.0 版][license]發行。請按一下[這裡][zip-download]，以取得含程式庫及所有相依性的 ZIP 檔案。此檔案由 Microsoft Open Technologies, Inc. 提供。關於授權及其他資訊，請參閱 ZIP 內的 license.txt 和 ThirdPartyNotices.txt 檔案。

## Azure Libraries for Java - Maven
如果您的專案已設定為使用 Maven 來建置，請將下列相依性新增至 pom.xml 檔案。注意：如需關於在 Eclipse (使用適用於 Java 的 Azure 程式庫) 中建立 Maven 專案的資訊，請參閱[開始使用 Java 的 Azure 管理程式庫][maven-getting-started]。

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-compute</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-network</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-sql</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-storage</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-management-websites</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-media</artifactId>
        <version>0.6.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus</artifactId>
        <version>0.7.0</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-serviceruntime</artifactId>
        <version>0.6.0</version>
    </dependency>


在 `<version>` 元素內，將此範例中的版本號碼換成有效的版本號碼，可以從 [Azure Libraries Repository on Maven](http://go.microsoft.com/fwlink/?LinkID=286274) 取得。

[license]: http://www.apache.org/licenses/LICENSE-2.0.html
[zip-download]: http://go.microsoft.com/fwlink/?LinkId=690320
[maven-getting-started]: http://go.microsoft.com/fwlink/?LinkID=622998

<!---HONumber=Oct15_HO3-->