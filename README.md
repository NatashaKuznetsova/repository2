# repository2
type
 PStack = ^TStack;
 TStack = record
   word: string;
   count: integer;
   next: PStack;
 end;

procedure Push(var stack: PStack; word: string);
var p, empty: PStack;
begin
 new(p);
 p^.word := word;
 p^.count := 1;
 p^.next := stack;

 if stack = nil then  
   stack := p
 else
 begin
   empty := p;
   while (p^.next <> nil) and (p^.next^.word < word) do p := p^.next;
   if p^.next = nil then 
   begin
     empty^.next := nil;
     p^.next := empty;
   end
   else
   begin  
     if (p^.next^.word = word) then
     begin
       p^.next^.count := p^.next^.count + 1;
       Dispose(empty);
     end
     else     
       begin 
        empty^.next := p^.next;
         p^.next := empty;
       end;
    end;
   end;
  end;

procedure Print(stack: PStack);
begin
 while stack <> nil do
 begin
   Writeln(stack^.word, '(', stack^.count, ')');
   stack := stack^.next;
 end;
end;

procedure Pop(var stack: PStack);
begin
 if stack <> nil then
 begin
   Pop(stack^.next);
   Dispose(stack);
   stack := nil;
 end;
end;

var
 f: TextFile;
 word: string;
 stack: PStack ;
begin
 AssignFile(f,'C:\Users\student340x\Desktop\gf.txt');
 Reset(f);
 while not Eof(f) do
 begin
   Readln(f, word);
   Push(stack, word);
 
 end;
 CloseFile(f);
 Print(stack);
 Pop(stack);
 Read;

end.
