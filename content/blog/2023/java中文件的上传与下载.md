---
title: "java中文件的上传与下载"
date: 2018-08-01 18:09:43
draft: false
---
文件上传:

upload.jsp
<%@ page language="java" contentType="text/html; charset=ISO-8859-1" pageEncoding="ISO-8859-1"%> <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd"> <html> <head> <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1"> <title>Insert title here</title> </head> <body> <form action="upload" method="post" enctype="multipart/form-data"> File: <input type="file" name="file"/> <input type="submit" value="Submit"/> </form> </body> </html>

Servlet: upload.java

protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException { DiskFileItemFactory factory = new DiskFileItemFactory(); factory.setSizeThreshold(1024 /* 500); File tempDirectory = new File("d:\\tempDirectory"); factory.setRepository(tempDirectory); ServletFileUpload upload = new ServletFileUpload(factory); upload.setSizeMax(1024 /* 1024 /* 5); try { List<FileItem> //* FileItem /*/items = upload.parseRequest(request); System.out.println(items); // 2. 遍历 items: for (FileItem item : items) { // 若是一个一般的表单域, 打印信息 if (item.isFormField()) { String name = item.getFieldName(); String value = item.getString(); System.out.println(name + ": " + value); } // 若是文件域则把文件保存到 d:\\files 目录下. else { String fieldName = item.getFieldName(); String fileName = item.getName(); String contentType = item.getContentType(); long sizeInBytes = item.getSize(); File filename=new File(fileName); fileName=filename.getName(); System.out.println(filename.getName()); System.out.println(contentType); System.out.println(sizeInBytes); InputStream in = null; try { in = item.getInputStream(); } catch (IOException e) { // TODO Auto-generated catch block e.printStackTrace(); } byte[] buffer = new byte[1024]; int len = 0; String fileName1 = "d:\\files\\" + fileName; System.out.println(fileName1); OutputStream out = null; try { out = new FileOutputStream(fileName1); } catch (FileNotFoundException e) { // TODO Auto-generated catch block e.printStackTrace(); } try { while ((len = in.read(buffer)) != -1) { out.write(buffer, 0, len); } } catch (IOException e) { // TODO Auto-generated catch block e.printStackTrace(); } try { out.close(); } catch (IOException e) { // TODO Auto-generated catch block e.printStackTrace(); } try { in.close(); } catch (IOException e) { // TODO Auto-generated catch block e.printStackTrace(); } } } } catch (FileUploadException e) { e.printStackTrace(); } }

文件下载:

download.jsp
<%@ page language="java" contentType="text/html; charset=ISO-8859-1" pageEncoding="ISO-8859-1"%> <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd"> <html> <head> <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1"> <title>Insert title here</title> </head> <body> <a href="download">download</a> </body> </html>

Servlet: download.java

protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException { response.setContentType("application/x-msdownload"); String fileName = "服务器.txt"; response.setHeader("Content-Disposition", "attachment;filename=" + URLEncoder.encode(fileName, "UTF-8")); OutputStream out = response.getOutputStream(); String pptFileName = "d:\\files\\服务器.txt"; InputStream in = new FileInputStream(pptFileName); byte [] buffer = new byte[1024]; int len = 0; while((len = in.read(buffer)) != -1){ out.write(buffer, 0, len); } in.close(); }