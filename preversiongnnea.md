gnn
===
//+------------------------------------------------------------------+
//|                                                          GNN.mq4 |
//|                        Copyright 2013, MetaQuotes Software Corp. |
//|                                        http://www.metaquotes.net |
//+------------------------------------------------------------------+
#property copyright "Copyright 2013, MetaQuotes Software Corp."
#property link      "http://www.metaquotes.net"
extern double SLP=60;
extern double TSP=30;
extern double TakeP=500;
extern int maxOpenPositions = 1;
//+------------------------------------------------------------------+
//| expert initialization function                                   |
//+------------------------------------------------------------------+
int init()
  {
//----
   
//----
   return(0);
  }
//+------------------------------------------------------------------+
//| expert deinitialization function                                 |
//+------------------------------------------------------------------+
int deinit()
  {
//----
   
//----
   return(0);
  }
//+------------------------------------------------------------------+
//| expert start function                                            |
//+------------------------------------------------------------------+
int start()
  {
  int  ticket, MMM, cnt, total;
  double adxp,adxm,cci,dm,fi,mom,rsi,rvim,rvis,stochm,stochs,wpr,mav,ishif,ishis,mfi,obv,ppp;
  double adxp1,adxm1,cci1,dm1,fi1,mom1,rsi1,rvim1,rvis1,stochm1,stochs1,wpr1,mav1,ishif1,ishis1,mfi1,obv1,ppp1;
  double adxpg,adxmg,ccig,dmg,fig,momg,rsig,rvimg,rvisg,stochmg,stochsg,wprg,mavg,ishifg,ishisg,mfig,obvg;
  double x, rviz,cciz, fiz,obvz,b,s,atr,b1,b2,b3,b4,b5,b6,s1,s2,s3,s4,s5,s6;
  double adxpp,adxmp,ccip,dmp,fip,momp,rsip,rvimp,rvisp,stochmp,stochsp,wprp,mavp,ishifp,ishisp,mfip,obvp;
  double adxpps,adxmps,ccips,dmps,fips,momps,rsips,rvimps,rvisps,stochmps,stochsps,wprps,mavps,ishifps,ishisps,mfips,obvps;
  atr=iATR(0,0,9,0);
  adxp=iADX(0,0,9,MODE_CLOSE,MODE_PLUSDI,0);
  adxm=iADX(0,0,9,MODE_CLOSE,MODE_MINUSDI,0);
  cci=iCCI(0,0,9,MODE_CLOSE,0);
  dm=iDeMarker(0,0,9,0);
  fi=iForce(0,0,9,0,0,0);
  mom=iMomentum(0,0,9,0,0);
  rsi=iRSI(0,0,9,0,0);
  rvim=iRVI(0,0,9,0,0);
  rvis=iRVI(0,0,9,1,0);
  stochm=iStochastic(0,0,9,3,3,0,1,0,0);
  stochs=iStochastic(0,0,9,3,3,0,1,1,0);
  wpr=iWPR(0,0,9,0);
  mav=iMA(0,0,9,0,0,0,0);
  ishif=iIchimoku(0,0,9,26,52,MODE_TENKANSEN,0);
  ishis=iIchimoku(0,0,9,26,52,MODE_KIJUNSEN,0);
  mfi=iMFI(0,0,9,0);
  obv=iOBV(0,0,0,0);
  //---- shift1-----
  adxp1=iADX(0,0,9,MODE_CLOSE,MODE_PLUSDI,1);
  adxm1=iADX(0,0,9,MODE_CLOSE,MODE_MINUSDI,1);
  cci1=iCCI(0,0,9,MODE_CLOSE,1);
  dm1=iDeMarker(0,0,9,1);
  fi1=iForce(0,0,9,0,0,1);
  mom1=iMomentum(0,0,9,0,1);
  rsi1=iRSI(0,0,9,0,1);
  rvim1=iRVI(0,0,9,0,1);
  rvis1=iRVI(0,0,9,1,1);
  stochm1=iStochastic(0,0,9,3,3,0,1,0,1);
  stochs1=iStochastic(0,0,9,3,3,0,1,1,1);
  wpr1=iWPR(0,0,9,1);
  mav1=iMA(0,0,9,0,0,0,1);
  ishif1=iIchimoku(0,0,9,26,52,MODE_TENKANSEN,1);
  ishis1=iIchimoku(0,0,9,26,52,MODE_KIJUNSEN,1);
  mfi1=iMFI(0,0,9,1);
  obv1=iOBV(0,0,0,1);
  
  //------preis-----
  
  ppp=(iClose(0,0,0)+iOpen(0,0,0))/2;
  ppp1=(iClose(0,0,1)+iOpen(0,0,1))/2;
  //--------gewichtungstep1
  x=(ppp/ppp1);
  adxpg=(adxp/adxp1);
  adxmg=(adxm/adxm1);
  cciz=(cci/cci1);
  //--------
  ccig=MathAbs(cciz);
  dmg=(dm/dm1);
  fiz=(fi/fi1);
  //------
  fig=MathAbs(fiz);
  momg=(mom/mom1);
  rsig=(rsi/rsi1);
  rvimg=(rvim/rvim1);
  rvisg=(rvis/rvis1);
  stochmg=(stochm/stochm1);
  stochsg=(stochs/stochs1);
  wprg=(wpr/wpr1);
  mavg=(mav/mav1);
  ishifg=(ishif/ishif1);
  ishisg=(ishis/ishis1);
  mfig=(mfi/mfi1);
  obvz=(obv/obv1);
  //---------
  obvg=MathAbs(obvz);
  //---gewichtung mit preis fur buy und s1-sn------
  if(x>=adxpg)
  {adxpp=adxpg/x;
  }
  else if(x<adxpg)
  {adxpp=x/adxpg;
  }
  else Print("ERRORadxpp");
//----
    if(x>=adxmg)
  {adxmp=adxmg/x;
  }
  else if(x<adxmg )
  {adxmp=x/adxmg;
  }
  else Print("ERRORadxmp");
//----
 if(x>=ccig)
  {ccip=ccig/x;
  }
  else if(x<ccig )
  {ccip=x/ccig;
  }
  else Print("ERRORccip");
//----
 if(x>=dmg)
  {dmp=dmg/x;
  }
  else if(x<dmg)
  {dmp=x/dmg;
  }
  else Print("ERRORdmp");
//----
 if(x>=fig)
  {fip=fig/x;
  }
  else if(x<fig )
  {fip=x/fig;
  }
  else Print("ERRORfip");
//----
 if(x>=momg)
  {momp=momg/x;
  }
  else if(x<momg)
  {momp=x/momg;
  }
  else Print("ERRORmomp");
//----
 if(x>=rsig)
  {rsip=rsig/x;
  }
  else if(x<rsig)
  {rsip=x/rsig;
  }
  else Print("ERRORrsip");
//----
 if(x>=rvimg)
  {rvimp=rvimg/x;
  }
  else if(x<rvimg)
  {rvimp=x/rvimg;
  }
  else Print("ERRORrvimp");
//---- 
if(x>=rvisg)
  {rvisp=rvisg/x;
  }
  else if(x<rvisg)
  {rvisp=x/rvisg;
  }
  else Print("ERRORrvisp");
//----
 if(x>=stochmg)
  {stochmp=stochmg/x;
  }
  else if(x<stochmg)
  {stochmp=x/stochmg;
  }
  else Print("ERRORstochmp");
//---- 
if(x>=stochsg)
  {stochsp=stochsg/x;
  }
  else if(x<stochsg )
  {stochsp=x/stochsg;
  }
  else Print("ERRORstochsp");
//---- 
if(x>=wprg)
  {wprp=wprg/x;
  }
  else if(x<wprg)
  {wprp=x/wprg;
  }
  else Print("ERRORwprp");
//---- 
if(x>=mavg)
  {mavp=mavg/x;
  }
  else if(x<mavg)
  {mavp=x/mavg;
  }
  else Print("ERRORmavg");
//---- 
if(x>=ishifg)
  {ishifp=ishifg/x;
  }
  else if(x<ishifg)
  {ishifp=x/ishifg;
  }
  else Print("ERRORishifp");
//---- 
if(x>=ishisg)
  {ishisp=ishisg/x;
  }
  else if(x<ishisg)
  {ishisp=x/ishisg;
  }
  else Print("ERRORishisp");
//---- 
if(x>=mfig)
  {mfip=mfig/x;
  }
  else if(x<mfig)
  {mfip=x/mfig;
  }
  else Print("ERRORmfip");
//---- 
if(x>=obvg)
  {obvp=obvg/x;
  }
  else if(x<obvg)
  {obvp=x/obvg;
  }
  else Print("ERRORobvp");
//-----------------------------------
//-------gewichtung fur sell seite---
if(x>=adxpg)
  {adxpps=1+(1-(adxpg/x));
  }
  else if(x<adxpg)
  {adxpps=1+(1-(x/adxpg));
  }
  else Print("ERRORadxpps");
//----
    if(x>=adxmg)
  {adxmps=1+(1-(adxmg/x));
  }
  else if(x<adxmg )
  {adxmps=1+(1-(x/adxmg));
  }
  else Print("ERRORadxmps");
//----
 if(x>=ccig)
  {ccips=1+(1-(ccig/x));
  }
  else if(x<ccig )
  {ccips=1+(1-(x/ccig));
  }
  else Print("ERRORccips");
//----
 if(x>=dmg)
  {dmps=1+(1-(dmg/x));
  }
  else if(x<dmg)
  {dmps=1+(1-(x/dmg));
  }
  else Print("ERRORdmps");
//----
 if(x>=fig)
  {fips=1+(1-(fig/x));
  }
  else if(x<fig )
  {fips=1+(1-(x/fig));
  }
  else Print("ERRORfips");
//----
 if(x>=momg)
  {momps=1+(1-(momg/x));
  }
  else if(x<momg)
  {momps=1+(1-(x/momg));
  }
  else Print("ERRORmomps");
//----
 if(x>=rsig)
  {rsips=1+(1-(rsig/x));
  }
  else if(x<rsig)
  {rsips=1+(1-(x/rsig));
  }
  else Print("ERRORrsips");
//----
 if(x>=rvimg)
  {rvimps=1+(1-(rvimg/x));
  }
  else if(x<rvimg)
  {rvimps=1+(1-(x/rvimg));
  }
  else Print("ERRORrvimps");
//---- 
if(x>=rvisg)
  {rvisps=1+(1-(rvisg/x));
  }
  else if(x<rvisg)
  {rvisps=1+(1-(x/rvisg));
  }
  else Print("ERRORrvisps");
//----
 if(x>=stochmg)
  {stochmps=1+(1-(stochmg/x));
  }
  else if(x<stochmg)
  {stochmps=1+(1-(x/stochmg));
  }
  else Print("ERRORstochmps");
//---- 
if(x>=stochsg)
  {stochsps=1+(1-(stochsg/x));
  }
  else if(x<stochsg )
  {stochsps=1+(1-(x/stochsg));
  }
  else Print("ERRORstochsps");
//---- 
if(x>=wprg)
  {wprps=1+(1-(wprg/x));
  }
  else if(x<wprg)
  {wprps=1+(1-(x/wprg));
  }
  else Print("ERRORwprps");
//---- 
if(x>=mavg)
  {mavps=1+(1-(mavg/x));
  }
  else if(x<mavg)
  {mavps=1+(1-(x/mavg));
  }
  else Print("ERRORmavgs");
//---- 
if(x>=ishifg)
  {ishifps=1+(1-(ishifg/x));
  }
  else if(x<ishifg)
  {ishifps=1+(1-(x/ishifg));
  }
  else Print("ERRORishifps");
//---- 
if(x>=ishisg)
  {ishisps=1+(1-(ishisg/x));
  }
  else if(x<ishisg)
  {ishisps=1+(1-(x/ishisg));
  }
  else Print("ERRORishisps");
//---- 
if(x>=mfig)
  {mfips=1+(1-(mfig/x));
  }
  else if(x<mfig)
  {mfips=1+(1-(x/mfig));
  }
  else Print("ERRORmfips");
//---- 
if(x>=obvg)
  {obvps=1+(1-(obvg/x));
  }
  else if(x<obvg)
  {obvps=1+(1-(x/obvg));
  }
  else Print("ERRORobvps");
  
  
//---standart geblubber
 //MONEY Management  
MMM=NormalizeDouble(AccountFreeMargin()/6/100,2);
 // Print("Account free margin = ",AccountFreeMargin());
                    if(Bars<100)
                         {
                         Print("bars less than 100");
                         return(0);  
                         }
                        total=OrdersTotal();
                    if(total<1) 
                       {
 // no opened orders identified
                    if(AccountFreeMargin()<(1000*0.004))
                         {
                          Print("We have no money. Free Margin = ", AccountFreeMargin());
                          return(0);  
                         }
                       }
//-----------------Normalize all!!



//--------Buy Signal-----
//---b1----
if(adxp>=adxm)
{b1=adxpg;}
else if(adxp<adxm)
{b1=1-((1-adxpp)*1.074);}
else Print("ERRORb1");
//---b2---
if(cci>=0)
{b2=ccig;}
else if(cci<0)
{b2=1-((1-ccip)*1.074);}
else Print("ERRORb2");
//---b3---dmg nicht!!
if(fi>=0)
{b3=fig;}
else if(fi<0)
{b3=1-((1-fip)*1.074);}
else Print("ERRORb3");
//---b4---
if(mom>=100)
{b4=momg;}
else if(mom<100)
{b4=1-((1-momp)*1.074);}
else Print("ERRORb4");
//---b5---rsi nicht!!!
if(rvim>=0 && rvim>=rvis)
{b5=rvimg;}
else if(rvim<0||rvim<rvis)
{b5=1-((1-rvimp)*1.074);}
else Print("ERRORb5");
//---b6---
if(stochm>=stochs)
{b6=stochmg;}
else if(stochm<stochs)
{b6=1-((1-stochmp)*1.074);}
else Print("ERRORb6");
//---b-fertig---wpr nicht!! mav auch nicht!!ishif und ishifs auch nicht!!mfi auch nicht! obv auch nicht!
//---ende anpassung buy signal phase
b=((adxpp*b1)+(ccip*b2)+(dmp*dmg)+(fip*b3)+(momp*b4)+(rsip*rsig)+(rvimp*b5)+(stochmp*b6)+(wprp*wprg)+(mavp*mavg)+(ishifp*ishifg)+(ishisp*ishisg)+(mfip*mfig)+(obvp*obvg))/14;
NormalizeDouble(b,7);
 if(Hour()>8 && Hour()<21)
          {
          //------loop fur maxposi
if(OrdersTotal() < maxOpenPositions)
{          
if (b>1)
               { 
                 ticket=OrderSend(Symbol(),OP_BUY,NormalizeDouble(AccountFreeMargin()/600,2),Ask,3,NormalizeDouble(Ask-(atr*2),5),0,"gnnggs buy",19219,0,Green);
                    if(ticket>0)
                        {
                         if(OrderSelect(ticket,SELECT_BY_TICKET,MODE_TRADES)) Print("BUY order opened : ",OrderOpenPrice());
                        }
                    else Print("Error opening BUY order : ",GetLastError()); 
                    return(0);
               }
}
//-----Sell Signal----------
//---s1----
if(adxm>=adxp)
{s1=adxmg;}
else if(adxm<adxp)
{s1=1+((1-adxmp)*1.074);}
else Print("ERRORs1");
//---s2---
if(cci<=0)
{s2=ccig;}
else if(cci>0)
{s2=1+((1-ccip)*1.074);}
else Print("ERRORs2");
//---s3---dmg nicht!!
if(fi<=0)
{s3=fig;}
else if(fi>0)
{s3=1+((1-fip)*1.074);}
else Print("ERRORs3");
//---s4---
if(mom<=100)
{s4=momg;}
else if(mom>100)
{s4=1+((1-momp)*1.074);}
else Print("ERRORs4");
//---s5---rsi nicht!!!
if(rvim<=0 && rvim<=rvis)
{s5=rvimg;}
else if(rvim>0||rvim>rvis)
{s5=1+((1-rvimp)*1.074);}
else Print("ERRORs5");
//---s6---
if(stochm<=stochs)
{s6=stochmg;}
else if(stochm>stochs)
{s6=1+((1-stochmp)*1.074);}
else Print("ERRORs6");
//---s-fertig---wpr nicht!! mav auch nicht!!ishif und ishifs auch nicht!!mfi auch nicht! obv auch nicht!
//---ende anpassung sell signal phase



s=((adxmps*s1)+(ccips*s2)+(dmps*dmg)+(fips*s3)+(momps*s4)+(rsips*rsig)+(rvimps*s5)+(stochmps*s6)+(wprps*wprg)+(mavps*mavg)+(ishifps*ishifg)+(ishisps*ishisg)+(mfips*mfig)+(obvps*obvg))/14;
NormalizeDouble(s,7);
if(OrdersTotal() < maxOpenPositions)
{
    if (s<1)
                  { 
                 ticket=OrderSend(Symbol(),OP_SELL,NormalizeDouble(AccountFreeMargin()/600,2),Bid,3,NormalizeDouble(Bid+(atr*2),5),0,"gnngs sell",19219,0,Green);
                  if(ticket>0)
                    {
                     if(OrderSelect(ticket,SELECT_BY_TICKET,MODE_TRADES)) Print("SELL order opened : ",OrderOpenPrice());
                    }
                  else Print("OrderSend sell failed with error # ",GetLastError());
                 return(0);
                 }
}
//------------------------
 for(cnt=0;cnt<total;cnt++)
    {
      OrderSelect(cnt, SELECT_BY_POS, MODE_TRADES);
      if(OrderType()<=OP_SELL &&      // check for opened position 
         OrderSymbol()==Symbol())    // check for symbol
          {
           if(OrderType()==OP_BUY)   // long position is opened
              {                     //check for Close
               if(b<1)
                  {
                   OrderClose(OrderTicket(),OrderLots(),Bid,3,Violet); // close position
                   return(0); // exit
                  }
//-------------------------------------------------
               if(atr>0)
                   {                 
                     if(Bid-OrderOpenPrice()>Point*(atr*20000))
                        {
                         if(OrderStopLoss()<Bid-Point*(atr*20000))
                            {
                              OrderModify(OrderTicket(),OrderOpenPrice(),Bid-Point*(atr*20000),OrderTakeProfit(),0,Green);
                              return(0);
                            }
                         }
                  }
             }
     
     //---------paran
   else 
      {    //for SHORT
           if(s>1)
             {
                OrderClose(OrderTicket(),OrderLots(),Ask,3,Violet); // close position
             return(0); // exit
             }
  // check for trailing stop
         if(atr>0)  
           {                 
            if((OrderOpenPrice()-Ask)>(Point*(atr*20000)))
                {
                if((OrderStopLoss()>(Ask+Point*(atr*20000))) || (OrderStopLoss()==0))
                     {
                       OrderModify(OrderTicket(),OrderOpenPrice(),Ask+Point*(atr*20000),OrderTakeProfit(),0,Red);
                    return(0);
                     }
                }
           }
     }//----close else
}    
}  //---close for-loop----
 }//-----trading Hour
     else Print("not the time to trade!");

//----
   return(0);
  }
//+------------------------------------------------------------------+
