# C++ tinyXML使用

# tinyXML下载： #

[http://sourceforge.net/projects/tinyxml/](http://sourceforge.net/projects/tinyxml/ "http://sourceforge.net/projects/tinyxml/")

# 加载到项目： #

这六个文件添加到你的c++工程中，分别是tinystr.h、tinystr.cpp、tinyxml.h、tinyxml.cpp、tinyxmlerror.cpp、tinyxmlparser.cpp

添加头文件

    #include "tinyxml.h"

# 使用： #


	_bstr_t errorWords;

	TiXmlDocument pXmlDoc;
	
	TiXmlDeclaration pDeclaration;
	pDeclaration.Parse( "<?xml version='1.0' encoding='UTF-8'?>", 0, TIXML_ENCODING_UNKNOWN );//插入头
	pXmlDoc.InsertEndChild(pDeclaration);
	TiXmlElement xElement("proof-result");
	TiXmlElement errorElement("error-result");

	TiXmlElement errorLevelElement("error");
	errorLevelElement.SetAttribute("level",szLevel);//设置节点属性
	TiXmlText  levelText(m_pCheckResult[i].ErrWord);
	levelText.SetCDATA(true);//设置DATA属性
	errorLevelElement.InsertEndChild(levelText);
	errorElement.InsertEndChild(errorLevelElement);
	TiXmlElement replaceElement("replace");

	TiXmlText  replaceText(errorWords);
	replaceText.SetCDATA(true);
	replaceElement.InsertEndChild(replaceText);//插入文本
	errorElement.InsertEndChild(replaceElement);
	TiXmlElement positionElement("position");

	TiXmlText  szLevelText(errorWords);
	positionElement.InsertEndChild(szLevelText);
	errorElement.InsertEndChild(positionElement);
	TiXmlElement source_sentenceElement("source_sentence");
	TiXmlText  sentenseText(sentense);
	sentenseText.SetCDATA(true);
	source_sentenceElement.InsertEndChild(sentenseText);
	errorElement.InsertEndChild(source_sentenceElement);
	xElement.InsertEndChild(errorElement);

	TiXmlElement leader_sort_errorsElement("leader_sort_errors");
	leader_sort_errorsElement.SetAttribute("count",szCount);
	TiXmlText  szLeaderBufferText((const char*)szLeaderBuffer);
	szLeaderBufferText.SetCDATA(true);
	leader_sort_errorsElement.InsertEndChild(szLeaderBufferText);
	xElement.InsertEndChild(leader_sort_errorsElement);

	pXmlDoc.InsertEndChild(xElement);//插入根节点

	//获得生成的xml字符串
	TiXmlPrinter printer;
	printer.SetStreamPrinting();
	pXmlDoc.Accept( &printer );
	_bstr_t bstrOutPutSentense(printer.CStr());


# 其他： #

可以查看tinyXML自带的文档