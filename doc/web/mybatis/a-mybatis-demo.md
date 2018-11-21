# A My Batis Demo

## 通过注解映射Mapper

```Java
import java.util.List;

import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.annotations.Select;

import com.sinosoft.ids.schema.VersionContent;

@Mapper
public interface VersionContentMapper {

	@Select("SELECT * FROM Version_Content WHERE versionNo = #{versionNo}")
    List<VersionContent> findByVersionNo(@Param("versionNo") String versionNo);

	@Select("SELECT count(*) FROM Version_Content WHERE versionNo = #{versionNo}")
	Integer countByVersionNo(@Param("versionNo") String versionNo);

}
```

## 通过Mapper配置文件使用Mybatis

通过Mapper配置文件使用Mybatis，需要指定Mapper文件路径，以及将Mapper文件和Service类映射起来，Mapper配置文件的路径不需要和`namespace`的一致

### SpringBoot中配置Mybatis的Mapper路径

```Pproperties
# mybatis
mybatis.mapper-locations=classpath:mapper/*.xml

```

### 通过Mapper配置文件和Service类映射

Mapper配置文件中有属性`namespace`可以指定对应的java类是哪个

## 参考

- [SpringBoot+Mybatis 整合 xml配置使用+免xml使用](https://blog.csdn.net/nanbiebao6522/article/details/80630302)
- [springboot集成mybatis xml方式](https://blog.csdn.net/lr131425/article/details/76269236)
- [Mybatis 框架使用的最核心内容（二）：mapper.xml中常用的标签详解](https://blog.csdn.net/qq_29233973/article/details/51433924)
