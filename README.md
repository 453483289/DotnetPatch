# DotnetPatch
���ڶ�dotnet�ļ��ķ��������滻���޸ġ�

�滻��ָ��ͬһ��Assembly�����һ����ķ������滻ָ�������ͬǩ����������ʱֻ�ǽ����޸ķ����ķ�����ָ���滻�ķ����壬ֻ�����ھ�̬�����벻����ʵ�������ķ�����
�ڽ����滻ǰͨ��Ӧ��ʹ��IlMerge���滻���뱻�滻���Assembly�ϲ���һ�𣨴˹���Ϊ΢������bin/Release�µ�IlMerge.exe���ǣ���

�Է������޸���Ҫ�ǲ������ָ���ʱͨ������dsl�ļ򵥽ű�������ʾ�����£�

proc(main)
{
	$files = getfilelist();
	begin("��ʼ�ű�����");
	looplist($files){
		$file=$$;
		beginfile($file,"��ʼ��"+$file+"�����޸ġ�����");
		beginreplace($file);
		replace($file,"Game","GamePatch");
		endreplace($file);
		beginmodify($file);
		writeloadarg($file,"PlayerController","Update",0x38,0);
		writeloadfield($file,"PlayerController","Update",0x39,"PlayerController","m_player");
		writeloadarg($file,"PlayerController","Update",0x3e,0);
		writeloadfield($file,"PlayerController","Update",0x3f,"PlayerController","m_moveElapseTime");
		writecall($file,"PlayerController","Update",0x44,"DebugConsoleHelper","Move");
		writenops($file,"PlayerController","Update",0x49,0x22);
		endmodify($file);
		endfile($file);
	};
	end("�����ű�����");
};

�ű�����ʱ����ȡ���������ӵĴ�������ļ���Ȼ�����ζԸ��ļ����д����ű�������Խ��з����滻����չĳ�������Ĵ�С���ĳ��������ָ������޸ġ�
�ű�֧�ֵ������У�

    begin(string tip);	//��ʼ�ű�����
    end(string tip);	//�����ű�����
    beginfile(string file, string tip);	//��ʼ�����ļ�
    endfile(string file);	//���������ļ�
    beginreplace(string file);	//��ʼ�������滻
    replace(string file, string srcclass, string repclass);	//�滻������
    endreplace(string file);	//�����������滻
    beginextend(string file);	//��ʼ��չ������С
    extend(string file, string classname, string methodname, uint insertsize); //��ָ���ķ�����ʼ����ָ���ֽ���
    endextend(string file);	//����������С��չ
    beginmodify(string file);	//��ʼ�ļ������޸�
    endmodify(string file);	//�����ļ������޸�
    
    writeloadarg(string file, string classname, string methodname, uint pos, int index);	//д��loadargָ��
    writeloadlocal(string file, string classname, string methodname, uint pos, int index);	//д��loadlocalָ��
    writeloadfield(string file, string classname, string methodname, uint pos, string loadClass, string loadField);	//д��loadfieldָ��
    writeloadstaticfield(string file, string classname, string methodname, uint pos, string loadClass, string loadField);	//д��loadstaticfieldָ��
    writecall(string file, string classname, string methodname, uint pos, string callClass, string callMethod);	//д��callָ��
    writecallvirt(string file, string classname, string methodname, uint pos, string callClass, string callMethod);	//д��callvirtָ��
    writenops(string file, string classname, string methodname, uint pos, int size);	//д����nopָ��
    
    getfilelist();	//��ȡ��ǰ��ӵĴ������ļ��б�
    log(format[,arg1,arg2,...]);	//�����־��������Ϣ���ڣ���ʽ������Ĺ�����string.Format������ͬ
    
���⣬����dsl�ļ򵥼���ű�֧���������

		���㣺+ - * / % () ?:
		�Ƚϣ�> >= < <= == !=
		�߼���&& || !
		������max min abs clamp
		��䣺if while loop looplist foreach
		λ�ñ�����������ʣ�arg(index) var(index)
		��ֵ���������� = ֵ�� �� ��var(index) = ֵ��
		
		if(�������ʽ){
			����б�;
		}elseif(�������ʽ){ //elseif������0�����
			����б�;		
		} else {	//else������0��1��
			����б�;
		};
		
		while(�������ʽ){
			����б�;
		};
		
		loop(����){
			$$ Ϊ��ǰ����
			����б�;
		};
		
		looplist(list){
			$$ Ϊ�б�������ĵ�ǰ����
			����б�;
		};
		
		foreach(e1,e2,...){
			$$ Ϊ�б�������ĵ�ǰ����
			����б�;
		};
		
ע�⣺�ű�����dslԪ���ԣ������﷨��ÿ������������Ҫ�ӷֺţ�