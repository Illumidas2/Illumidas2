<bean id="vendorProperties" class="org.springframework.beans.factory.config.PropertiesFactoryBean">
    <property name="properties">
        <props>
            <prop key="SQL Server">sqlserver</prop>
            <prop key="DB2">db2</prop>
            <prop key="Oracle">oracle</prop>
            <prop key="MySQL">mysql</prop>
        </props>
    </property>
</bean>

<bean id="databaseIdProvider" class="org.apache.ibatis.mapping.VendorDatabaseIdProvider">
    <property name="properties" ref="vendorProperties"/>
</bean>

<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource" />
    <property name="mapperLocations" value="classpath*:sample/config/mappers/**/*.xml" />
    <property name="databaseIdProvider" ref="databaseIdProvider"/>
</bean>