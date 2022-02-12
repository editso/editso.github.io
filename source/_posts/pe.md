---
title: PE头文件学习笔记
date: 2021-11-05 13:01:22
tags:
	- PE
	- Windows
categories:
  - Windows
  - PE
---

## [可移植可执行](https://zh.wikipedia.org/wiki/%E5%8F%AF%E7%A7%BB%E6%A4%8D%E5%8F%AF%E6%89%A7%E8%A1%8C)
1. 是一种用于可执行文件、目标文件和动态链接库的文件格式，主要使用在32位和64位的Windows操作系统上。`可移植的`是指该文件格式的通用性，可用于许多种不同的操作系统和体系结构中。PE文件格式封装了Windows操作系统加载可执行程序代码时所必需的一些信息。这些信息包括动态链接库、API导入和导出表、资源管理数据和线程局部存储数据。在Windows NT操作系统中，PE文件格式主要用于EXE文件、DLL文件、.sys（驱动程序）和其他文件类型。可扩展固件接口（EFI）技术规范书中说明PE格式是EFI环境中的标准可执行文件格式。开头为DOS头部。
2. PE格式是由Unix中的COFF格式修改而来的。在Windows开发环境中，PE格式也称为PE/COFF格式。


## [DOS头](https://docs.microsoft.com/en-us/windows/win32/debug/pe-format#ms-dos-stub-image-only) `winnt.h`
### 结构体信息
```
typedef struct _IMAGE_DOS_HEADER {      // DOS .EXE header
    WORD   e_magic;                     // * Magic number 
    WORD   e_cblp;                      // Bytes on last page of file
    WORD   e_cp;                        // Pages in file
    WORD   e_crlc;                      // Relocations
    WORD   e_cparhdr;                   // Size of header in paragraphs
    WORD   e_minalloc;                  // Minimum extra paragraphs needed
    WORD   e_maxalloc;                  // Maximum extra paragraphs needed
    WORD   e_ss;                        // Initial (relative) SS value
    WORD   e_sp;                        // Initial SP value
    WORD   e_csum;                      // Checksum
    WORD   e_ip;                        // Initial IP value
    WORD   e_cs;                        // Initial (relative) CS value
    WORD   e_lfarlc;                    // File address of relocation table
    WORD   e_ovno;                      // Overlay number
    WORD   e_res[4];                    // Reserved words
    WORD   e_oemid;                     // OEM identifier (for e_oeminfo)
    WORD   e_oeminfo;                   // OEM information; e_oemid specific
    WORD   e_res2[10];                  // Reserved words
    LONG   e_lfanew;                    // * File address of new exe header
} IMAGE_DOS_HEADER, *PIMAGE_DOS_HEADER;

```
### 重要字段说明
1. **e_magic**: 4D5A
    该字段标识是否是一个PE文件以
2. **e_lfanew**: --
    指向 NT头(**IMAGE_NT_HEADER**) 起始地址

## NT头
NT在64和32结构没有发生变法, 只是字段类型长度有变
### 结构体信息
#### [x86](https://docs.microsoft.com/en-us/windows/win32/api/winnt/ns-winnt-image_nt_headers32)
```
typedef struct _IMAGE_NT_HEADERS {
    DWORD Signature;
    IMAGE_FILE_HEADER FileHeader;
    IMAGE_OPTIONAL_HEADER32 OptionalHeader;
} IMAGE_NT_HEADERS32, *PIMAGE_NT_HEADERS32;
```
#### [x64](https://docs.microsoft.com/en-us/windows/win32/api/winnt/ns-winnt-image_nt_headers64)
```
typedef struct _IMAGE_NT_HEADERS64 {
  DWORD                   Signature;
  IMAGE_FILE_HEADER       FileHeader;
  IMAGE_OPTIONAL_HEADER64 OptionalHeader;
} IMAGE_NT_HEADERS64, *PIMAGE_NT_HEADERS64;
```

### 重要字段说明
1. **Signature**: 5045 
    4字节PE标识

## [文件头](https://docs.microsoft.com/en-us/windows/win32/api/winnt/ns-winnt-image_file_header)

### 结构体信息
```
typedef struct _IMAGE_FILE_HEADER {
  WORD  Machine;              //  IMAGE_FILE_MACHINE_I386(0x014c) , IMAGE_FILE_MACHINE_IA64(0x0200), IMAGE_FILE_MACHINE_AMD64(0x8664)
  WORD  NumberOfSections;     // *
  DWORD TimeDateStamp;        // - 
  DWORD PointerToSymbolTable; // -
  DWORD NumberOfSymbols;      // - 
  WORD  SizeOfOptionalHeader; // *
  WORD  Characteristics;      // *
} IMAGE_FILE_HEADER, *PIMAGE_FILE_HEADER;
``` 

### 重要字段说明 
1. **NumberOfSections**
    节数量

2. **SizeOfOptionalHeader**
    可选PE头的大小

3. **Characteristics**
    属性信息

## 可选PE头

### 结构体信息
#### [x86](https://docs.microsoft.com/en-us/windows/win32/api/winnt/ns-winnt-image_optional_header32)
```
typedef struct _IMAGE_OPTIONAL_HEADER {
  WORD                 Magic;
  BYTE                 MajorLinkerVersion;
  BYTE                 MinorLinkerVersion;
  DWORD                SizeOfCode;
  DWORD                SizeOfInitializedData;
  DWORD                SizeOfUninitializedData;
  DWORD                AddressOfEntryPoint;
  DWORD                BaseOfCode;
  DWORD                BaseOfData;
  DWORD                ImageBase;
  DWORD                SectionAlignment;
  DWORD                FileAlignment;
  WORD                 MajorOperatingSystemVersion;
  WORD                 MinorOperatingSystemVersion;
  WORD                 MajorImageVersion;
  WORD                 MinorImageVersion;
  WORD                 MajorSubsystemVersion;
  WORD                 MinorSubsystemVersion;
  DWORD                Win32VersionValue;
  DWORD                SizeOfImage;
  DWORD                SizeOfHeaders;
  DWORD                CheckSum;
  WORD                 Subsystem;
  WORD                 DllCharacteristics;
  DWORD                SizeOfStackReserve;
  DWORD                SizeOfStackCommit;
  DWORD                SizeOfHeapReserve;
  DWORD                SizeOfHeapCommit;
  DWORD                LoaderFlags;
  DWORD                NumberOfRvaAndSizes;
  IMAGE_DATA_DIRECTORY DataDirectory[IMAGE_NUMBEROF_DIRECTORY_ENTRIES];
} IMAGE_OPTIONAL_HEADER32, *PIMAGE_OPTIONAL_HEADER32;
```
#### [x64](https://docs.microsoft.com/en-us/windows/win32/api/winnt/ns-winnt-image_optional_header64)
```
typedef struct _IMAGE_OPTIONAL_HEADER64 {
  WORD                 Magic;                       // *
  BYTE                 MajorLinkerVersion;          // -
  BYTE                 MinorLinkerVersion;          // -
  DWORD                SizeOfCode;                  // -
  DWORD                SizeOfInitializedData;       // -
  DWORD                SizeOfUninitializedData;     // -
  DWORD                AddressOfEntryPoint;         // *
  DWORD                BaseOfCode;                  // -
  ULONGLONG            ImageBase;                   // *
  DWORD                SectionAlignment;            // *
  DWORD                FileAlignment;               // *
  WORD                 MajorOperatingSystemVersion; // -
  WORD                 MinorOperatingSystemVersion; // -
  WORD                 MajorImageVersion;           // -
  WORD                 MinorImageVersion;           // -
  WORD                 MajorSubsystemVersion;       // -
  WORD                 MinorSubsystemVersion;       // -
  DWORD                Win32VersionValue;           // -
  DWORD                SizeOfImage;                 // *
  DWORD                SizeOfHeaders;               // *
  DWORD                CheckSum;                    // -
  WORD                 Subsystem;                   // -
  WORD                 DllCharacteristics;          // -
  ULONGLONG            SizeOfStackReserve;          // -
  ULONGLONG            SizeOfStackCommit;           // -
  ULONGLONG            SizeOfHeapReserve;           // -
  ULONGLONG            SizeOfHeapCommit;            // -
  DWORD                LoaderFlags;                 // -
  DWORD                NumberOfRvaAndSizes;         // -
  IMAGE_DATA_DIRECTORY DataDirectory[IMAGE_NUMBEROF_DIRECTORY_ENTRIES];
} IMAGE_OPTIONAL_HEADER64, *PIMAGE_OPTIONAL_HEADER64;
```

### 重要字段说明
1. **Magic**:
    - IMAGE_NT_OPTIONAL_HDR32_MAGIC: 0x10b 
        32位可执行文件
    - IMAGE_NT_OPTIONAL_HDR64_MAGIC: 0x20b
        64位可执行文件
    - IMAGE_ROM_OPTIONAL_HDR_MAGIC: 0x107
        ROM文件

2. **AddressOfEntryPoint**:
    存储入口函数偏移, 该值只是一个偏移, 真实地址需要加上 **ImageBase**

3. **ImageBase**:
    - 在内存中加载的首地址, 该值是`64K`字节的倍数
    - DLL 的默认值为 0x10000000。应用程序的默认值为0x00400000，但0x00010000的 Windows CE 除外

4. **SectionAlignment**:
    - 在内存中加载后节表应该按多少字节对齐

5. **FileAlignment**:
    - 在文件中应该按多少字节对齐, 默认按`512K`对齐

6. **SizeOfImage**:
    - 加载到内存中的总大小

7. **SizeOfHeaders**:
    - PE头总大小, **包括节表**

## [节表](https://docs.microsoft.com/en-us/windows/win32/api/winnt/ns-winnt-image_section_header)
节表中定义了相关数据开始与结束信息 *(如: 代码段, 数据段, ...)*, 它很重要,但节表中的值都是RAV 也就是拉伸后的`偏移地址`
### 结构体信息
```
typedef struct _IMAGE_SECTION_HEADER {
  BYTE  Name[IMAGE_SIZEOF_SHORT_NAME];  // *
  union {                              
    DWORD PhysicalAddress;              
    DWORD VirtualSize;                  
  } Misc;                               // *
  DWORD VirtualAddress;                 // *
  DWORD SizeOfRawData;                  // *
  DWORD PointerToRawData;               // *
  DWORD PointerToRelocations;           // -
  DWORD PointerToLinenumbers;           // -
  WORD  NumberOfRelocations;            // -
  WORD  NumberOfLinenumbers;            // -
  DWORD Characteristics;                // *
} IMAGE_SECTION_HEADER, *PIMAGE_SECTION_HEADER;
```
### 重要字段说明
1. **Name**:
    节表名称, 注意它不会以0结尾, 所以在获取节表名称时需要注意
2. **Misc**:
    它在内存中加载后的大小
3. **VirtualAddress**: 
    内存中的偏移`(RVA)`
4. **SizeOfRawData**:
    节区数据大小, `Misc` 中的数据可能比它大
5.  **PointerToRawData**:
    文件偏移地址`(FOV)`

## [目录表](https://docs.microsoft.com/en-us/windows/win32/api/winnt/ns-winnt-image_data_directory)
目录表有16个, 分别为:
1. IMAGE_DIRECTORY_ENTRY_ARCHITECTURE: 7  
    Architecture-specific data
2. IMAGE_DIRECTORY_ENTRY_BASERELOC: 5   
    Base relocation table
3. IMAGE_DIRECTORY_ENTRY_BOUND_IMPORT: 11  
    Bound import directory
4. IMAGE_DIRECTORY_ENTRY_COM_DESCRIPTOR: 14  
    COM descriptor table
5. IMAGE_DIRECTORY_ENTRY_DEBUG: 6  
    Debug directory
6. IMAGE_DIRECTORY_ENTRY_DELAY_IMPORT: 13  
    Delay import table
7. IMAGE_DIRECTORY_ENTRY_EXCEPTION: 3  
    Exception directory
8. IMAGE_DIRECTORY_ENTRY_EXPORT: 0  
    Export directory
9. IMAGE_DIRECTORY_ENTRY_GLOBALPTR: 8    
    The relative virtual address of global pointer
10. IMAGE_DIRECTORY_ENTRY_IAT: 12  
    Import address table
11. IMAGE_DIRECTORY_ENTRY_IMPORT: 1  
    Import directory
12. IMAGE_DIRECTORY_ENTRY_LOAD_CONFIG: 10  
    Load configuration directory
13. IMAGE_DIRECTORY_ENTRY_RESOURCE: 2  
    Resource directory
14. IMAGE_DIRECTORY_ENTRY_SECURITY: 4  
    Security directory
15. IMAGE_DIRECTORY_ENTRY_TLS: 9  
    Thread local storage directory  

### 结构体信息
```
typedef struct _IMAGE_DATA_DIRECTORY {
  DWORD VirtualAddress; // *
  DWORD Size;
} IMAGE_DATA_DIRECTORY, *PIMAGE_DATA_DIRECTORY;
```

### 重要字段信息说明
1. **VirtualAddress**:
    指向各表偏移`(RVA)`

### 目标表相关结构体
这里只列出 导出表,  导入表, 重定位表

#### 导出表
导出表比较复杂, 需要理解导出方式与查找函数首地址

##### 结构体信息
``` // 导出表
typedef struct _IMAGE_EXPORT_DIRECTORY {
    DWORD   Characteristics;
    DWORD   TimeDateStamp;
    WORD    MajorVersion;
    WORD    MinorVersion;
    DWORD   Name;
    DWORD   Base;
    DWORD   NumberOfFunctions;
    DWORD   NumberOfNames;
    DWORD   AddressOfFunctions;     // RVA from base of image
    DWORD   AddressOfNames;         // RVA from base of image
    DWORD   AddressOfNameOrdinals;  // RVA from base of image
} IMAGE_EXPORT_DIRECTORY, *PIMAGE_EXPORT_DIRECTORY;
```

##### 重要字段说明
1. **Name**:  
    导出文件名称偏移地址 `(RVA)`
2. **Base**:  
    需要开始
3. **NumberOfFunctions**:  
    导出函数数量
4. **NumberOfNames**:  
    以名称导出的函数数量
5. **AddressOfFunctions**:  
    导出的函数存放首地址偏移 `(RVA)`
6. **AddressOfNames**:  
    以名称导出的函数存放首地址偏移`(RVA)`
7. **AddressOfNameOrdinals**:  
    以名称导出的函数指向的序号地址偏移 `(RVA)`

##### 打印`32位`导出表信息列子
```
#include <Windows.h>
#include <winnt.h>
#include <stdio.h>

DWORD RvaToFov(PIMAGE_SECTION_HEADER sec, DWORD secCount, DWORD rva) {

	for (int i = 0; i < secCount; i++) {
		
		if (sec->VirtualAddress <= rva && sec->VirtualAddress + sec->SizeOfRawData >= rva) {
			return rva - sec->VirtualAddress + sec->PointerToRawData;
		}
		
		sec++;
	}

	return 0;
}


void printExportTable(const char* dllName) {
	LPBYTE lpBuffer = NULL;
	HANDLE hFile = NULL;
	DWORD fileSize = 0, readSize = 0, secCount = 0;
	LPDWORD lpExportFunctions = NULL, lpExportFunctionNames = NULL;
	LPWORD lpExportFunctionOrdinals = NULL;
	PIMAGE_DOS_HEADER lpDosHeader = NULL;
	PIMAGE_NT_HEADERS32 lpNtHeader = NULL;
	PIMAGE_SECTION_HEADER lpFirstSection = NULL;
	PIMAGE_EXPORT_DIRECTORY lpExport = NULL;

	hFile = CreateFileA(dllName, GENERIC_READ, FILE_SHARE_READ, NULL, OPEN_ALWAYS, FILE_ATTRIBUTE_NORMAL, NULL);

	if (!hFile) {
		printf("读取%s失败\n", dllName);
		goto exit;
	}

	fileSize = GetFileSize(hFile, NULL);

	lpBuffer = (LPBYTE)VirtualAlloc(NULL, fileSize, MEM_COMMIT, PAGE_READWRITE);

	if (!lpBuffer) {
		printf("分配内存失败 %d\n", fileSize);
		goto exit;
	}
	
	if (!ReadFile(hFile, lpBuffer, fileSize, &readSize, NULL)) {
		goto exit;
	}

	lpDosHeader = (PIMAGE_DOS_HEADER)lpBuffer;
	
	if (lpDosHeader->e_magic != IMAGE_DOS_SIGNATURE) {
		printf("不是一个有效的PE文件\n");
		goto exit;
	}

	lpNtHeader = (PIMAGE_NT_HEADERS32)(lpBuffer + lpDosHeader->e_lfanew);
	

	if (lpNtHeader->Signature != IMAGE_NT_SIGNATURE) {
		printf("不是一个有效的PE文件\n");
		goto exit;
	}

	if (lpNtHeader->OptionalHeader.Magic != IMAGE_NT_OPTIONAL_HDR32_MAGIC) {
		printf("不是一个32位PE文件\n");
		goto exit;
	}
	
	secCount = lpNtHeader->FileHeader.NumberOfSections;
	lpFirstSection = (PIMAGE_SECTION_HEADER)((LPBYTE)&lpNtHeader->OptionalHeader + lpNtHeader->FileHeader.SizeOfOptionalHeader);
	
	lpExport = lpBuffer + RvaToFov(
			lpFirstSection, 
			secCount, 
			lpNtHeader->OptionalHeader.DataDirectory[IMAGE_DIRECTORY_ENTRY_EXPORT].VirtualAddress
	);

	// 导出函数
	lpExportFunctions = (LPDWORD)(lpBuffer + RvaToFov(
		lpFirstSection,
		secCount,
		lpExport->AddressOfFunctions
	));

	// 导出函数名称
	lpExportFunctionNames = (LPDWORD)(lpBuffer + RvaToFov(
		lpFirstSection,
		secCount,
		lpExport->AddressOfNames
	));

	// 导出序号
	lpExportFunctionOrdinals = (LPWORD)(lpBuffer + RvaToFov(
		lpFirstSection,
		secCount,
		lpExport->AddressOfNameOrdinals
	));

	printf("导出文件名称: %s\n", lpBuffer + RvaToFov(lpFirstSection, secCount, lpExport->Name));
	printf("导出函数总数: 0x%x\n", lpExport->NumberOfFunctions);
	printf("导出函数名称总数: 0x%x\n", lpExport->NumberOfNames);

	for (int i = 0, j = 0; i < lpExport->NumberOfFunctions; i++, j = 0) {
		
		for (; j < lpExport->NumberOfNames; j++) {
			if (i == lpExportFunctionOrdinals[j]) {
				break;
			}
		}
		
		if (j < lpExport->NumberOfNames) {
			printf("\n以函数名称导出: \n\t名称: %s\n\t地址: 0x%x\n\t序号: 0x%x\n", lpBuffer + RvaToFov(
					lpFirstSection,
					secCount,
					lpExportFunctionNames[j]
				),
				lpExportFunctions[i],
				lpExportFunctionOrdinals[j] + lpExport->Base
			);
		}else {
			printf("\n以序号导出: \n\t地址: 0x%x\n\t序号: 0x%x\n", lpExportFunctions[i], i + lpExport->Base);
		}
	}

exit:
	if (hFile) {
		CloseHandle(hFile);
	}
	if (lpBuffer) {
		VirtualFree(lpBuffer, 0, MEM_RELEASE);
	}
}
```

#### 导入表
导入表也是比较复杂的, 需要理解导入方式

##### 相关结构体信息
###### 导入表
```
typedef struct _IMAGE_IMPORT_DESCRIPTOR {
    union {
        DWORD   Characteristics;            // 0 for terminating null import descriptor
        DWORD   OriginalFirstThunk;         // RVA to original unbound IAT (PIMAGE_THUNK_DATA)
    } DUMMYUNIONNAME;
    DWORD   TimeDateStamp;                  // 0 if not bound,
                                            // -1 if bound, and real date\time stamp
                                            //     in IMAGE_DIRECTORY_ENTRY_BOUND_IMPORT (new BIND)
                                            // O.W. date/time stamp of DLL bound to (Old BIND)

    DWORD   ForwarderChain;                 // -1 if no forwarders
    DWORD   Name;
    DWORD   FirstThunk;                     // RVA to IAT (if bound this IAT has actual addresses)
} IMAGE_IMPORT_DESCRIPTOR;
typedef IMAGE_IMPORT_DESCRIPTOR UNALIGNED *PIMAGE_IMPORT_DESCRIPTOR;
```

###### 名称表
```
typedef struct _IMAGE_IMPORT_BY_NAME {
    WORD    Hint;
    CHAR   Name[1];
} IMAGE_IMPORT_BY_NAME, *PIMAGE_IMPORT_BY_NAME;
```

###### 导入名称表与导入地址表

```
typedef struct _IMAGE_THUNK_DATA32 {
    union {
        DWORD ForwarderString;      // PBYTE 
        DWORD Function;             // PDWORD
        DWORD Ordinal;
        DWORD AddressOfData;        // PIMAGE_IMPORT_BY_NAME
    } u1;
} IMAGE_THUNK_DATA32;
```

###### 重要字段说明
- **导入表**
    1. **DUMMYUNIONNAME**:  
        指向导入名称表的偏移`(RVA)`
    2. **Name**:  
        导入动态库的名称
    3. **FirstThunk**:  
        指向导入地址表的偏移`(RVA)`, 导入地址表在文件中是和导入名称表一样的, 但加载到内存后是不一样的

- **导入名称表与导入地址表**
    1. **u1**:        
        指向的是函数名称的偏移或者序号,
        如果是函数明显那么最高位为`0`否则为序号  
        *序号 = ul.Ordinal & ~(0b1 << 31)*

- **名称表**
    1. **Name**
        函数名称

##### 打印`32位`导入表列子
```
#include <Windows.h>
#include <winnt.h>
#include <stdio.h>


DWORD RvaToFov(PIMAGE_SECTION_HEADER sec, DWORD secCount, DWORD rva) {

	for (int i = 0; i < secCount; i++) {
		
		if (sec->VirtualAddress <= rva && sec->VirtualAddress + sec->SizeOfRawData >= rva) {
			return rva - sec->VirtualAddress + sec->PointerToRawData;
		}
		
		sec++;
	}

	return 0;
}


void printImportTable(const char* peFileName) {
	DWORD fileSize = 0, readSize = 0, secCount = 0;
	LPBYTE lpBuffer = NULL;
	HANDLE hFile = NULL;
	PIMAGE_DOS_HEADER lpDosHeader = NULL; // dos头
	PIMAGE_NT_HEADERS32 lpNtHeader = NULL; // nt头
	PIMAGE_SECTION_HEADER lpFirstSection = NULL; // 节表
	
	PIMAGE_THUNK_DATA32 
		lpIntHeader = NULL, // 导入名称表 
		lpIatHeader = NULL; // 导入地址表

	PIMAGE_IMPORT_BY_NAME lpImportName = NULL;
	PIMAGE_IMPORT_DESCRIPTOR lpImport = NULL;
	

	hFile = CreateFileA(peFileName, GENERIC_READ, FILE_SHARE_READ, NULL, OPEN_ALWAYS, FILE_ATTRIBUTE_NORMAL, NULL);

	if (!hFile) {
		printf("读取%s失败\n", peFileName);
		goto exit;
	}

	fileSize = GetFileSize(hFile, NULL);

	lpBuffer = (LPBYTE)VirtualAlloc(NULL, fileSize, MEM_COMMIT, PAGE_READWRITE);

	if (!lpBuffer) {
		printf("分配内存失败 %d\n", fileSize);
		goto exit;
	}

	if (!ReadFile(hFile, lpBuffer, fileSize, &readSize, NULL)) {
		goto exit;
	}

	lpDosHeader = (PIMAGE_DOS_HEADER)lpBuffer;

	if (lpDosHeader->e_magic != IMAGE_DOS_SIGNATURE) {
		printf("不是一个有效的PE文件\n");
		goto exit;
	}

	lpNtHeader = (PIMAGE_NT_HEADERS32)(lpBuffer + lpDosHeader->e_lfanew);


	if (lpNtHeader->Signature != IMAGE_NT_SIGNATURE) {
		printf("不是一个有效的PE文件\n");
		goto exit;
	}

	if (lpNtHeader->OptionalHeader.Magic != IMAGE_NT_OPTIONAL_HDR32_MAGIC) {
		printf("不是一个32位PE文件\n");
		goto exit;
	}

	secCount = lpNtHeader->FileHeader.NumberOfSections;
	lpFirstSection = (PIMAGE_SECTION_HEADER)((LPBYTE)&lpNtHeader->OptionalHeader + lpNtHeader->FileHeader.SizeOfOptionalHeader);


	lpImport = (PIMAGE_IMPORT_DESCRIPTOR)(lpBuffer + RvaToFov(
		lpFirstSection, 
		secCount, 
		lpNtHeader->OptionalHeader.DataDirectory[IMAGE_DIRECTORY_ENTRY_IMPORT].VirtualAddress)
	);

	while (lpImport->Name != 0) {
		
		printf("\n 导入动态库名称: %s\n", (lpBuffer + RvaToFov(
			lpFirstSection,
			secCount,
			lpImport->Name
		)));

		lpIntHeader = (PIMAGE_THUNK_DATA32)(lpBuffer + RvaToFov(
			lpFirstSection,
			secCount,
			lpImport->OriginalFirstThunk
		));
	
		lpIatHeader = (PIMAGE_THUNK_DATA32)(lpBuffer + RvaToFov(
			lpFirstSection,
			secCount,
			lpImport->FirstThunk
		));
		
		while (lpIntHeader->u1.Function != 0) {
		
			if (lpIatHeader->u1.Function >> 31 == 1) {
				printf("\t序号导入: 0x%x\n", lpIatHeader->u1.Ordinal & ~(0b1 << 31));
			} else {
				lpImportName = (PIMAGE_IMPORT_BY_NAME)(lpBuffer + RvaToFov(
					lpFirstSection,
					secCount,
					lpIatHeader->u1.Function
				));

				printf("\t名称导入: %s\n", lpImportName->Name);
			}
	
			lpIntHeader++;
			lpIatHeader++;
		}
		
		lpImport++;
	}
	
exit:

	if (hFile) {
		CloseHandle(hFile);
	}

	if (lpBuffer) {
		VirtualFree(lpBuffer, 0, MEM_RELEASE);
	}
	
}
```

#### 重定位表

##### 结构体信息
```
typedef struct _IMAGE_BASE_RELOCATION {
    DWORD   VirtualAddress;
    DWORD   SizeOfBlock;
//  WORD    TypeOffset[1];
} IMAGE_BASE_RELOCATION;
typedef IMAGE_BASE_RELOCATION UNALIGNED * PIMAGE_BASE_RELOCATION;
```
##### 重要字段说明
1. **VirtualAddress**:
    偏移基址
2. **SizeOfBlock**:
    当前表大小

##### 打印`32位`重定位表列子
```
#include <Windows.h>
#include <winnt.h>
#include <stdio.h>

DWORD RvaToFov(PIMAGE_SECTION_HEADER sec, DWORD secCount, DWORD rva) {

	for (int i = 0; i < secCount; i++) {
		
		if (sec->VirtualAddress <= rva && sec->VirtualAddress + sec->SizeOfRawData >= rva) {
			return rva - sec->VirtualAddress + sec->PointerToRawData;
		}
		
		sec++;
	}

	return 0;
}

void printRelocTable(const char* peFileName) {
	DWORD fileSize = 0, readSize = 0, secCount = 0;
	LPBYTE lpBuffer = NULL;
	HANDLE hFile = NULL;
	LPWORD lpRelocAddress = NULL;
	PIMAGE_DOS_HEADER lpDosHeader = NULL; // dos头
	PIMAGE_NT_HEADERS32 lpNtHeader = NULL; // nt头
	PIMAGE_SECTION_HEADER lpFirstSection = NULL; // 节表
	PIMAGE_BASE_RELOCATION lpRelocHeader = NULL;

	hFile = CreateFileA(peFileName, GENERIC_READ, FILE_SHARE_READ, NULL, OPEN_ALWAYS, FILE_ATTRIBUTE_NORMAL, NULL);

	if (!hFile) {
		printf("读取%s失败\n", peFileName);
		goto exit;
	}

	fileSize = GetFileSize(hFile, NULL);

	lpBuffer = (LPBYTE)VirtualAlloc(NULL, fileSize, MEM_COMMIT, PAGE_READWRITE);

	if (!lpBuffer) {
		printf("分配内存失败 %d\n", fileSize);
		goto exit;
	}

	if (!ReadFile(hFile, lpBuffer, fileSize, &readSize, NULL)) {
		goto exit;
	}

	lpDosHeader = (PIMAGE_DOS_HEADER)lpBuffer;

	if (lpDosHeader->e_magic != IMAGE_DOS_SIGNATURE) {
		printf("不是一个有效的PE文件\n");
		goto exit;
	}

	lpNtHeader = (PIMAGE_NT_HEADERS32)(lpBuffer + lpDosHeader->e_lfanew);


	if (lpNtHeader->Signature != IMAGE_NT_SIGNATURE) {
		printf("不是一个有效的PE文件\n");
		goto exit;
	}

	if (lpNtHeader->OptionalHeader.Magic != IMAGE_NT_OPTIONAL_HDR32_MAGIC) {
		printf("不是一个32位PE文件\n");
		goto exit;
	}

	secCount = lpNtHeader->FileHeader.NumberOfSections;
	lpFirstSection = (PIMAGE_SECTION_HEADER)((LPBYTE)&lpNtHeader->OptionalHeader + lpNtHeader->FileHeader.SizeOfOptionalHeader);

	lpRelocHeader = (PIMAGE_BASE_RELOCATION)(lpBuffer + RvaToFov(
		lpFirstSection,
		secCount,
		lpNtHeader->OptionalHeader.DataDirectory[IMAGE_DIRECTORY_ENTRY_BASERELOC].VirtualAddress)
	);

	while (lpRelocHeader->SizeOfBlock != 0) {
	
		printf("\n重定位修复: ");
		printf("\n\t基址(RVA): 0x%x", lpRelocHeader->VirtualAddress);
		printf("\n\t块儿大小: 0x%x", lpRelocHeader->SizeOfBlock);
		printf("\n\t需要重定位数量: 0x%x", (lpRelocHeader->SizeOfBlock - 8) / 2);
	
		lpRelocAddress = (LPWORD)((LPBYTE)lpRelocHeader + 8);

		for (int i = 0; i < (lpRelocHeader->SizeOfBlock - 8) / 2; i++) {
			
			if (lpRelocAddress[i] >> 12 == 3) {
				DWORD addrOffset = lpRelocAddress[i] & 0xFFF;
				DWORD addrValue = *((DWORD *)(lpBuffer + RvaToFov(
					lpFirstSection,
					secCount,
					addrOffset + lpRelocHeader->VirtualAddress
				)));

				printf("\n\t地址(RVA): 0x%x",  addrOffset + lpRelocHeader->VirtualAddress);
				printf("\n\t当前值: 0x%x", addrValue);
				printf("\n\t偏移: 0x%x\n", addrValue - lpNtHeader->OptionalHeader.ImageBase);
			}
		}
	
		lpRelocHeader = (PIMAGE_BASE_RELOCATION)((LPBYTE)lpRelocHeader + lpRelocHeader->SizeOfBlock);
	}

exit:

	if (hFile) {
		CloseHandle(hFile);
	}

	if (lpBuffer) {
		VirtualFree(lpBuffer, 0, MEM_RELEASE);
	}
}

```
