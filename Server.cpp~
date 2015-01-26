#include "SharedObject.h"
#include "Semaphore.h"
#include "thread.h"
#include "socketserver.h"
#include <stdlib.h>
#include <time.h>
#include <list>
#include <pthread.h>
#include <vector>

class CommThread : public Thread
{
private:
    int threadid;
    Socket theSocket;
    Socket theSocket2;
public:
    CommThread(int newthreadid, Socket const & p, Socket const & p2)
        : Thread(false),threadid(newthreadid), theSocket(p), theSocket2(p2)
    {
        ;
    }
    long ThreadMain(void)
    {
        ByteArray bytes;
        std::cout << "Created a socket thread!" << std::endl;
	
	std::string wait = "wait"; 
	clock_t t; 
	int f; 
	t = clock(); 		


        for(;;)
        {
	    t = clock() - t; 
            //if ((((float)t)/CLOCKS_PER_SEC) < (1/144))
	    //{
		//changing the y 
		//read stuff from socket
		ByteArray bytes;

		std::string socket1String; 
		std::string socket2String; 

	    	int read = theSocket.Read(bytes);

		if (read == -1)
            	{
                	std::cout << "Error in socket detected" << std::endl;
                	break;
            	}
            	else if (read == 0)
            	{
                	std::cout << "Socket closed at remote end" << std::endl;
                	break;
            	}	
            	else
            	{
                	socket1String = bytes.ToString();
                	std::cout << "Received: " << socket1String << std::endl;
            	}
		
		//read stuff from socket2
		ByteArray bytes2;
		int read2 = theSocket2.Read(bytes2); 

		if (read2 == -1)
            	{
                	std::cout << "Error in socket detected" << std::endl;
                	break;
            	}
            	else if (read2 == 0)
            	{
                	std::cout << "Socket closed at remote end" << std::endl;
                	break;
            	}	
            	else
            	{
                	socket2String = bytes2.ToString();
                	std::cout << "Received: " << socket2String << std::endl;
            	}
		
		//write stuff 
		ByteArray ba(socket2String);      
	
	    	int written2 = theSocket.Write(ba); 
            	if ( written2 != ba.v.size())
            	{
                	std::cout << "Wrote: " << written2 << std::endl;
                	std::cout << "The socket appears to have been closed" << std::endl;
                	break;
            	}
            	else
            	{
                	std::cout << "Sent players to client1: " << socket2String << std::endl;
            	}

		//write more stuff 
		ByteArray ba2(socket1String);      
	
	    	int written3 = theSocket2.Write(ba2); 
            	if ( written3 != ba2.v.size())
            	{
                	std::cout << "Wrote: " << written3 << std::endl;
                	std::cout << "The socket appears to have been closed" << std::endl;
                	break;
            	}
            	else
            	{
                	std::cout << "Sent players to client1: " << socket1String << std::endl;
            	}

	    //}

        }
        std::cout << "Thread is gracefully ending" << std::endl;
	//theSocket.Close(); 
	//theSocket2.Close(); 
	
    }
    ~CommThread(void)
    {
        theSocket.Close();
	theSocket2.Close(); 
        terminationEvent.Wait();
    }
};

class KillThread  : public Thread
{
private:
    SocketServer & theServer;
public:
    KillThread(SocketServer & t)
        : Thread(true),theServer(t)
    {
        ;
    }
    long ThreadMain(void)
    {
        std::cout << "Type 'quit' when you want to close the server" << std::endl;
        for (;;)
        {
            std::string s;
            std::cin >> s;
            if(s=="quit")
            {
                theServer.Shutdown();
                break;
            }
        }
    }
};



int main(void)
{
    std::cout << "I am a socket server" << std::endl;
    SocketServer theServer(2000);
    KillThread killer(theServer);
    std::vector<CommThread *> threads;

    //variable declarations
    int threadId = 0 ; 
    std::string onePlayer = "1";
    std::string twoPlayer = "2";  
    


    for(;;)
    {
        try
        {
	    //player1
	    //send the number of players (onePlayer) 
            Socket newSocket = theServer.Accept();
	     	    
            ByteArray ba(onePlayer);      
	
	    int written = newSocket.Write(ba); 
            if ( written != ba.v.size())
            {
                std::cout << "Wrote: " << written << std::endl;
                std::cout << "The socket appears to have been closed" << std::endl;
                break;
            }
            else
            {
                std::cout << "Sent players to client1: " << onePlayer << std::endl;
            }

	    //read from the client socket1
	    ByteArray bytes;
	    int read = newSocket.Read(bytes);
            if (read == -1)
            {
                std::cout << "Error in socket detected" << std::endl;
                break;
            }
            else if (read == 0)
            {
                std::cout << "Socket closed at remote end" << std::endl;
                break;
            }
            else
            {
                std::string theString = bytes.ToString();
                std::cout << "Received: " << theString << std::endl;
            }


	    



            //step 3: send 2 to client 1
	    std::string wait = "2";
	    ByteArray ba2(wait);
            
            int written2 = newSocket.Write(ba2);
            if ( written2 != ba2.v.size())
            {
                std::cout << "Wrote: " << written2 << std::endl;
                std::cout << "The socket appears to have been closed" << std::endl;
                break;
            }
            else
            {
                std::cout << "Sent 2 to client 1: " << wait << std::endl;
            }

	    
	    //receive response from client 1 after step 3 
	    //ByteArray bytes;
	    /*
	    read = newSocket.Read(bytes);
            if (read == -1)
            {
                std::cout << "Error in socket detected" << std::endl;
                break;
            }
            else if (read == 0)
            {
                std::cout << "Socket closed at remote end" << std::endl;
                break;
            }
            else
            {
                std::string theString = bytes.ToString();
                std::cout << "Received: " << theString << std::endl;
            }
	    */

	    
	    //step4: accept client 2
	    Socket newSocket2 = theServer.Accept();

	    //step 5: send count to client 1 and client 2
	    //sending player count to client 1
	    ByteArray ba3(twoPlayer); 
	    
	    written = newSocket.Write(ba3); 
            if ( written != ba3.v.size())
            {
                std::cout << "Wrote: " << written << std::endl;
                std::cout << "The socket appears to have been closed" << std::endl;
                break;
            }
            else
            {
                std::cout << "Sent playercount to client1: " << onePlayer << std::endl;
            }
	    
	    //read from the client socket1
	    read = newSocket.Read(bytes);
            if (read == -1)
            {
                std::cout << "Error in socket detected" << std::endl;
                break;
            }
            else if (read == 0)
            {
                std::cout << "Socket closed at remote end" << std::endl;
                break;
            }
            else
            {
                std::string theString = bytes.ToString();
                std::cout << "Received: " << theString << std::endl;
            }


	    //sending player count to client2
	    ByteArray ba4(twoPlayer); 

	    written = newSocket2.Write(ba4); 
            if ( written != ba4.v.size())
            {
                std::cout << "Wrote: " << written << std::endl;
                std::cout << "The socket appears to have been closed" << std::endl;
                break;
            }
            else
            {
                std::cout << "Sent player count to client2: " << twoPlayer << std::endl;
            }

	    //read from the client 2
	    read = newSocket2.Read(bytes);
            if (read == -1)
            {
                std::cout << "Error in socket detected" << std::endl;
                break;
            }
            else if (read == 0)
            {
                std::cout << "Socket closed at remote end" << std::endl;
                break;
            }
            else
            {
                std::string theString = bytes.ToString();
                std::cout << "Received: " << theString << std::endl;
            }



		


	    //step 6: send 3 to client 1 and client 2 
	    //send it to client 1 first 
	    ByteArray ba5("3"); 
	    written = newSocket.Write(ba5);
	
	    if ( written != ba5.v.size())
            {
                std::cout << "Wrote: " << written << std::endl;
                std::cout << "The socket appears to have been closed" << std::endl;
                break;
            }
            else
            {
                std::cout << "Sent code 3 to player1" << std::endl;
            }

		/*
	      
	    //read from the client socket1
	    read = newSocket.Read(bytes);
            if (read == -1)
            {
                std::cout << "Error in socket detected" << std::endl;
                break;
            }
            else if (read == 0)
            {
                std::cout << "Socket closed at remote end" << std::endl;
                break;
            }
            else
            {
                std::string theString = bytes.ToString();
                std::cout << "Received: " << theString << std::endl;
            }
		*/


	    //now send it to client 2 
	    written = newSocket2.Write(ba5);
	
	    if ( written != ba5.v.size())
            {
                std::cout << "Wrote: " << written << std::endl;
                std::cout << "The socket appears to have been closed" << std::endl;
                break;
            }
            else
            {
                std::cout << "Sent code 3 to client 2: " << std::endl;
            }
	    
	    /*
	    //read from the client socket2
	    read = newSocket2.Read(bytes);
            if (read == -1)
            {
                std::cout << "Error in socket detected" << std::endl;
                break;
            }
            else if (read == 0)
            {
                std::cout << "Socket closed at remote end" << std::endl;
                break;
            }
            else
            {
                std::string theString = bytes.ToString();
                std::cout << "Received: " << theString << std::endl;
            }
	     */

	    //socket connection is done 
            std::cout << "Received a socket connection!" << std::endl;
            //threads.push_back(new CommThread(newSocket));
	    //threads.push_back(new CommThread(newSocket2)); 
	    
            //passes the sockets to the thread after games started so coordinates can be passes
	    CommThread *newconnection = new CommThread(threadId++, newSocket, newSocket2); 
	    newconnection->Start();
	    threads.push_back(newconnection); 

	    

	    	
	    
	    

	}
        catch(TerminationException e)
        {
            std::cout << "The socket server is no longer listening. Exiting now." << std::endl;
            break;
        }
        catch(std::string s)
        {
            std::cout << "thrown " << s << std::endl;
            break;
        }
        catch(...)
        {
            std::cout << "caught  unknown exception" << std::endl;
            break;
        }
    }
	
	
            
        
    std::cout << "End of main" << std::endl;
    for (int i=0;i<threads.size();i++)
        delete threads[i];
}
