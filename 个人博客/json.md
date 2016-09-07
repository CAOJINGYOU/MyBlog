# Json

# JsonCpp #

## 下载： ##

[https://github.com/open-source-parsers/jsoncpp](https://github.com/open-source-parsers/jsoncpp "https://github.com/open-source-parsers/jsoncpp")

[https://sourceforge.net/projects/jsoncpp/](https://sourceforge.net/projects/jsoncpp/ "https://sourceforge.net/projects/jsoncpp/")

[https://github.com/open-source-parsers/jsoncpp/archive/1.7.4.zip](https://github.com/open-source-parsers/jsoncpp/archive/1.7.4.zip "JsonCpp1.7.4")

## 使用： ##

### 配置： ###

本人使用的是JsonCpp1.7.4，解压后直接把include与src复制到自己的项目下。

在项目中附加包含目录：`../include;../src/lib_json;`

把两个目录中的文件添加到项目中

json_reader.cpp、json_value.cpp和json_writer.cpp三个文件的预编译头改成“不使用预编译头”

### 代码： ###

    Json::Value root;   // will contains the root value after parsing.
    Json::Reader reader;
    bool parsingSuccessful = reader.parse( config_doc, root );
    if ( !parsingSuccessful )
    {
    // report to the user the failure and their locations in the document.
    std::cout  << "Failed to parse configuration\n"
       << reader.getFormattedErrorMessages();
    return;
    }
    
    // Get the value of the member of root named 'encoding', return 'UTF-8' if there is no
    // such member.
    std::string encoding = root.get("encoding", "UTF-8" ).asString();
    // Get the value of the member of root named 'encoding', return a 'null' value if
    // there is no such member.
    const Json::Value plugins = root["plug-ins"];
    for ( int index = 0; index < plugins.size(); ++index )  // Iterates over the sequence elements.
    loadPlugIn( plugins[index].asString() );
    
    setIndentLength( root["indent"].get("length", 3).asInt() );
    setIndentUseSpace( root["indent"].get("use_space", true).asBool() );
    
    // ...
    // At application shutdown to make the new configuration document:
    // Since Json::Value has implicit constructor for all value types, it is not
    // necessary to explicitly construct the Json::Value object:
    root["encoding"] = getCurrentEncoding();
    root["indent"]["length"] = getCurrentIndentLength();
    root["indent"]["use_space"] = getCurrentIndentUseSpace();
    
    Json::StyledWriter writer;
    // Make a new JSON document for the configuration. Preserve original comments.
    std::string outputConfig = writer.write( root );
    
    // You can also use streams.  This will put the contents of any JSON
    // stream at a particular sub-value, if you'd like.
    std::cin >> root["subtree"];
    
    // And you can write to a stream, using the StyledWriter automatically.
    std::cout << root;


# ggicci--json #

发现JsonCpp竟然有内存泄露！！！可能我用法不对，放弃了。另外找了一个ggicci--json。

## 下载地址： ##

[https://github.com/ggicci/ggicci--json](https://github.com/ggicci/ggicci--json "https://github.com/ggicci/ggicci--json")

# rapidjson #

rapidjson更快,最终使用了rapidjson

## 下载地址： ##

[https://github.com/miloyip/rapidjson/](https://github.com/miloyip/rapidjson/ "https://github.com/miloyip/rapidjson/")

## 使用方法： ##

	#include "rapidjson/document.h"
	using namespace rapidjson;
	
	CString strTemp = _T("{\"form\": \"sysAdmin\",\"to\":\"caoyh\",\"msg\": \"NewPic\"}");
	
	CString strForm, strTo;
	
	Document json;
	json.Parse(strTemp);
	strForm = json[_T("form")].GetString();
	strTo = json[_T("to")].GetString();
	strMsg = json[_T("msg")].GetString();